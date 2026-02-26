<img src="Screenshot from 2024-12-09 14-44-11.png" width="450" align="left">














#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kode|Kode]]

## Introduction
Ada case misal dimana header nya tidak terisi atau kosong. Kita bisa isi secara manual lohhh di datatable nya.

## Kode
```javascript
function getData(){
    $("#table").DataTable({
        ordering: false,
        dom: "<'row'<'col-sm-6 text-left'f><'col-sm-6 text-right'B>>\\\\
        <'row'<'col-sm-12'tr>>\\\\
        <'row'<'col-sm-12 col-md-5'i><'col-sm-12 col-md-7 dataTables_pager'lp>>",
        buttons: [
            {
                extend: 'excel',
                filename: 'Laporan Serah Terima Sampel Noodle Harian',
                exportOptions: {
                    columns: ':not(:last-child)',
                    format: {
                        header: function (data, columnIdx) {
                            switch (columnIdx) {
                                case 0:
                                    return 'MID';
                                case 1:
                                    return 'Varian';
                                case 2:
                                    return 'Tgl Produksi';
                                case 3:
                                    return 'Tokiwa';
                                case 4:
                                    return 'Jumlah Sampel';
                                default:
                                    return data;
                            }
                        }
                    }
                }
            },
        ],
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

            $('.dataTables_filter').hide();

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
- **`dom`**: Opsi ini mengatur struktur HTML yang digunakan oleh DataTables untuk menampilkan elemen-elemen seperti filter, pagination, dan tabel itu sendiri.
- **`<row>`**: Menandakan bahwa kita menggunakan baris untuk mengatur elemen.
- **`<col-sm-6 text-left>`**: Mengatur kolom untuk filter (`f`) di sebelah kiri.
- **`<col-sm-6 text-right>`**: Mengatur kolom untuk tombol (`B`) di sebelah kanan.
- **`<tr>`**: Menandakan tempat di mana tabel data akan ditampilkan.
- **`<col-sm-12 col-md-5>`**: Mengatur kolom untuk informasi tabel (`i`).
- **`<col-sm-12 col-md-7 dataTables_pager`**: Mengatur kolom untuk pagination (`p`).
- **`buttons`**: Opsi ini mendefinisikan tombol-tombol yang akan ditampilkan di DataTable, dalam hal ini, tombol untuk mengekspor data ke Excel.
- **`extend: 'excel'`**: Menentukan bahwa tombol ini akan digunakan untuk mengekspor data ke format Excel.
- **`filename: 'Laporan Serah Terima Sampel Noodle Harian'`**: Menentukan nama file yang akan digunakan saat mengunduh file Excel.
- **`exportOptions`**: Opsi ini mengatur bagaimana data akan diekspor.
    - **`columns: ':not(:last-child)'`**: Menentukan kolom mana yang akan diekspor. Dalam hal ini, semua kolom kecuali kolom terakhir akan diekspor.
    - **`format`**: Opsi ini mengatur format data yang akan diekspor.
        - **`header`**: Fungsi ini digunakan untuk menentukan header kustom untuk setiap kolom saat diekspor.
            - **`data`**: Parameter ini berisi data header asli dari kolom.
            - **`columnIdx`**: Parameter ini berisi indeks kolom yang sedang diproses.
            - **`switch (columnIdx)`**: Menggunakan pernyataan `switch` untuk menentukan header kustom berdasarkan indeks kolom.
                - **`case 0`**: Jika kolom pertama (indeks 0), header yang akan digunakan adalah 'MID'.
                - **`case 1`**: Jika kolom kedua (indeks 1), header yang akan digunakan adalah 'Varian'.
                - **`case 2`**: Jika kolom ketiga (indeks 2), header yang akan digunakan adalah 'Tgl Produksi'.
                - **`case 3`**: Jika kolom keempat (indeks 3), header yang akan digunakan adalah 'Tokiwa'.
                - **`case 4`**: Jika kolom kelima (indeks 4), header yang akan digunakan adalah 'Jumlah Sampel'.
                - **`default`**: Jika tidak ada kecocokan, mengembalikan data asli dari header.

Date: 10-12-2024