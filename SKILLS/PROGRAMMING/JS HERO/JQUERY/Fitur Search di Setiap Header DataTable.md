![[Screenshot from 2024-12-09 14-23-13.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kode|Kode]]

## Introduction
Daripada milih satu satu mending bisa di search semua. Mempermudah ketika export data.

## Kode
```javascript
function getData(){
    $("#table").DataTable({
        ordering: false,
        ajax: {
            type: "GET",
            url: "/shelf-life-qa/report/getDataSerahTerimaHarian"
        },
        columns: [
            { data: "master_variant.mid" },
            { data: "master_variant.kode" },
            { data: "tgl_produksi" },
            { data: "tokiwa" },
            {
                data: "jumlah_sample",
                width: "10%",
                render: function(data, type, row){
                    return `<center>${data}</center>`
                }
            },
            {
                data: "status",
                render: function(data, type, row){
                    let status = ''
                    if(data == null){
                        status = `<span class="badge bg-secondary">waiting check</span>`;
                    }else if(data == 'diterima'){
                        status = `<span class="badge bg-success text-white">diterima</span>`;
                    }else if(data == 'check in'){
                        status = `<span class="badge bg-info text-white">check in</span>`;
                    }else if(data == 'approve'){
                        status = `<span class="badge text-white" style="background-color: blue">approve</span>`;
                    }else{
                        status = `<span class="badge bg-danger text-white">reject</span>`;
                    }

                    return status;
                }
            },
        ],
        lengthMenu: [
            [10, 25, 50, -1],
            [10, 25, 50, "Semua"]
        ],
        pageLength: 10,
        initComplete: function() {
            var api = this.api();
            api.columns([0, 1, 2, 3, 4, 5]).eq(0).each(function(colIdx) {
                var cell = api.column(colIdx).header();
                var title = $(cell).text();
                $(cell).html('<input type="text" class="form-control" placeholder="' + title + '" />');
                $($(cell).find('input')).on('keyup change', function() {
                    api.column(colIdx).search(this.value).draw();
                });
            });
        }
    });
} getData();
```

Penjelasan:
- **Fungsi**: `api.columns([0, 1, 2, 3, 4, 5])` digunakan untuk memilih kolom-kolom tertentu dari tabel. Dalam hal ini, kolom yang dipilih adalah kolom dengan indeks 0 hingga 5 (total 6 kolom).
- **`eq(0)`**: Mengembalikan kolom yang dipilih dalam urutan yang sama seperti yang ditentukan. Ini memastikan bahwa kita dapat melakukan iterasi melalui kolom-kolom tersebut.
- **`each(function(colIdx) { ... });`**: Melakukan iterasi untuk setiap kolom yang dipilih, di mana `colIdx` adalah indeks kolom saat ini dalam iterasi.

Date: 09-12-2024