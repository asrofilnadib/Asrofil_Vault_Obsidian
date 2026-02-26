#Tech 
## Introduction
Ketika gue pegang project CPAR Audit gue menemukan banyak data yg double seperti gambar berikut.

![[Pasted image 20250515210306.png]]
Saya ingin yg kolom nya sama harusnya tampil 1x saja. Caranya yaitu menggunakan `json.data.filter` di Datatable

## Implementasi

```javascript
function table(filter_dept, filter_reference, filter_issued_date){
    let seen = new Set();

    $("#table").DataTable({
        serverSide: true,
        bDestroy: true,
        ordering: false,
        ajax: {
            type: "GET",
            url: "/cpar-audit/dcc/internal/checklist-cpar/getData",
            data: {
                filter_dept, filter_reference, filter_issued_date
            },
            dataSrc: function(json) {
                return json.data.filter(function(item) {
                    const key = JSON.stringify([
                        item.iso,
                        item.dept,
                        item.internal?.creator_nik,
                        item.internal?.creator_nama,
                        moment(item.created_at).format("YYYY-MM-DD")
                    ]);

                    if(seen.has(key)) {
                        return false;
                    }

					seen.add(key);

                    return true;
                });
            }
        },
        columns: [
            { data: "iso" },
            { data: "dept" },
            { data: "internal.creator_nik" },
            { data: "internal.creator_nama" },
            {
                data: "created_at",
                render: function(data, type, row){
                    return moment(data).format("YYYY-MM-DD");
                }
            },
            {
                render: function(data) {
                    return `
                    <button class="btn btn-success"><i class="fas fa-eye"></i></button>
                    `;
                }
            }
        ]
    });

} table(filter_dept, filter_reference, filter_issued_date);
```

Penjelasan:
- `dataSrc`: Fungsi ini memodifikasi respons dari server sebelum DataTable render.
- `seen = new Set()`: Membantu melacak key unik untuk tiap baris.
- Key dibuat berdasarkan kombinasi:
    - `item.iso`
    - `item.dept`
    - `item.internal.creator_nik`
    - `item.internal.creator_nama`
    - `created_at` diformat ke `YYYY-MM-DD`

Date: 15-05-2025