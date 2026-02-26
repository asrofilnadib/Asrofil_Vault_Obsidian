#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kode|Kode]]
- [[#Hasil|Hasil]]

## Introduction
Di project Shelf Life QA ada kebutuhan untuk menampilkan semua data agar bisa export semuanya. Berikut cara nya:

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
            { data: "status" },
        ],
        lengthMenu: [
            [10, 25, 50, -1],
            [10, 25, 50, "Semua"]
        ],
        pageLength: 10
    });
} getData();
```

Penjelasan:
- **Fungsi**: `lengthMenu` digunakan untuk menentukan jumlah entri yang ditampilkan dalam tabel. Ini adalah dropdown yang memungkinkan pengguna untuk memilih berapa banyak baris yang ingin mereka lihat.
- **Format**:
    - Array pertama (`[10, 25, 50, -1]`) berisi nilai-nilai yang akan digunakan untuk jumlah entri yang ditampilkan.
        - `10`, `25`, dan `50` adalah jumlah baris yang dapat dipilih.
        - `1` menunjukkan bahwa pengguna dapat memilih untuk melihat "Semua" entri.
    - Array kedua (`[10, 25, 50, "Semua"]`) berisi label yang akan ditampilkan di dropdown.
        - Label "Semua" akan ditampilkan untuk opsi `1`.

## Hasil
<img src="Screenshot from 2024-12-09 14-13-41 1.png" align="left">










Date: 09-12-2024