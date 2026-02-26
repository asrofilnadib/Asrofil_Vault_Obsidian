![[Searching Spesific Column.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Codingan nya|Codingan nya]]
- [[#Kode Lengkapnya|Kode Lengkapnya]]

## Introduction
Gue dapat saran pencarian di datatable harus spesifik.

<img src="Searching Spesific Column 2.png" width="250" align="left">













User gue maunya kalau dia ngetik 1 ya munculnya 1 bukan 10. Kenapa seperti itu?

DataTables secara default menggunakan **pencarian substring,** yang berarti bahwa ketika kita mengetik "1", sistem akan mencocokkan semua nilai yang mengandung "1" di dalamnya, termasuk "10", "11", "12", dan seterusnya.

Untuk mengatasi masalah ini, kita dapat menggunakan pencarian berbasis regex yang memungkinkan pencarian yang lebih ketat. Dengan regex, kita dapat memastikan bahwa hanya nilai yang persis sama dengan yang dicari yang akan ditampilkan.

## Codingan nya
```php
initComplete: function() {
    var api = this.api();

    $('.dataTables_filter').hide();
    
    api.columns([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21]).eq(0).each(function(colIdx) {
        var cell = api.column(colIdx).header();
        var title = $(cell).text();
        $(cell).html('<input type="text" class="form-control" placeholder="' + title + '" />');
        
        $($(cell).find('input')).on('keyup change', function() {
            if (colIdx === 20) { 
                var searchValue = this.value;
                api.column(colIdx).search('^' + searchValue + '$', true, false).draw();
            } else {
                api.column(colIdx).search(this.value).draw();
            }
        });
    });
}
```

Disini kita menggunakan regex ^$ yg artinya kita memperketat pencarian!

`api.column(colIdx).search('^' + searchValue + '$', true, false).draw();`

## Kode Lengkapnya
```javascript
function tableHasil(){
    $("#tableHasil").DataTable({
        processing: true,
        ordering: false,
        paging: false,
        bDestroy: true,
        // dom: "<'row'<'col-sm-6 text-left'B><'col-sm-6 text-right'l>>\\
        // <'row'<'col-sm-12'tr>>\\
        // <'row'<'col-sm-12 col-md-5'i>>",
        dom: "<'row'<'col-sm-6 text-left'B><'col-sm-6 text-right'l>>\\
        <'row'<'col-sm-12'tr>>\\
        <'row'<'col-sm-12 col-md-5'i><'col-sm-12 col-md-7'p>>",
        buttons: [
            {
                extend: 'excel',
                filename: 'Laporan Hasil Analisis',
                exportOptions: {
                    format: {
                        header: function (data, columnIdx) {
                            switch (columnIdx) {
                                case 0:
                                    return 'MID';
                                case 1:
                                    return 'Varian';
                                case 2:
                                    return 'Plan';
                                case 3:
                                    return 'Tahun';
                                case 4:
                                    return 'Bulan';
                                case 5:
                                    return 'Kadar Air Mie';
                                case 6:
                                    return 'AV';
                                case 7:
                                    return 'Aroma Mie';
                                case 8:
                                    return 'Kadar Air Powder';
                                case 9:
                                    return 'Fisik';
                                case 10:
                                    return 'Kadar Air Garnish';
                                case 11:
                                    return 'Aroma Garnish';
                                case 12:
                                    return 'Fisik Saos';
                                case 13:
                                    return 'Fisik Kecap';
                                case 14:
                                    return 'Fisik Sayur';
                                case 15:
                                    return 'Aroma Oil';
                                case 16:
                                    return 'TPC';
                                case 17:
                                    return 'Yeast & Mould';
                                case 18:
                                    return 'Ecoli';
                                case 19:
                                    return 'Salmonella';
                                case 20:
                                    return 'Umur';
                                case 21:
                                    return 'Alasan Reject';
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
            url: "/shelf-life-qa/report/getDataHasilAnalisis",
        },
        columns: [
            { 
                data: "master_variant.mid",
                render: function(data, type, row){
                    return `
                    <button class="btn btn-success">${data ?? ''}</button>`
                }
            },
            { data: "master_variant.kode" },
            { data: "sampel_bulanan.plan" },
            { 
                data: "tgl_produksi",
                render: function(data, type, row){
                    return moment(data).format("YYYY");
                }
            },
            { 
                data: "tgl_produksi",
                render: function(data, type, row){
                    return moment(data).format("MMMM");
                }
            },
            { 
                data: "mie_kadar_air",
                render: function(data, type, row){
                    return (typeof data === 'number' && !isNaN(data)) ? data.toFixed(2) : '';
                }
            },
            { 
                data: "mie_av",
                render: function(data, type, row){
                    return (typeof data === 'number' && !isNaN(data)) ? data.toFixed(4) : '';
                }
            },
            { data: "mie_aroma" },
            { 
                data: "powder_kadar_air",
                render: function(data, type, row){
                    return (typeof data === 'number' && !isNaN(data)) ? data.toFixed(2) : '';
                }
            },
            { data: "powder_fisik" },
            { 
                data: "garnish_kadar_air",
                render: function(data, type, row){
                    return (typeof data === 'number' && !isNaN(data)) ? data.toFixed(2) : '';
                }
            },
            { data: "garnish_aroma" },
            { data: "saos_fisik" },
            { data: "kecap_fisik" },
            { data: "sayur_fisik" },
            { data: "oil_aroma" },
            { data: "parameter_mie_bumbu_tpc" },
            { data: "parameter_mie_bumbu_yeast_mould" },
            { data: "parameter_mie_bumbu_ecoli" },
            { data: "parameter_mie_bumbu_salmonella" },
            // { data: "parameter_mie_bumbu_mikrobiologi_1" },
            // { data: "parameter_mie_bumbu_mikrobiologi_2" },
            { data: "umur" },
            { data: "alasan_reject" },
        ],
        lengthMenu: [
            [10, 25, 50, 100, -1], 
            [10, 25, 50, 100, "Semua"] 
        ],
        pageLength: 10, 
        initComplete: function() {
            var api = this.api();

            $('.dataTables_filter').hide();
            
            api.columns([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21]).eq(0).each(function(colIdx) {
                var cell = api.column(colIdx).header();
                var title = $(cell).text();
                $(cell).html('<input type="text" class="form-control" placeholder="' + title + '" />');
                
                $($(cell).find('input')).on('keyup change', function() {
                    if (colIdx === 20) { 
                        var searchValue = this.value;
                        api.column(colIdx).search('^' + searchValue + '$', true, false).draw();
                    } else {
                        api.column(colIdx).search(this.value).draw();
                    }
                });
            });
        }
    });
} tableHasil();
```

Date: 05-03-2025