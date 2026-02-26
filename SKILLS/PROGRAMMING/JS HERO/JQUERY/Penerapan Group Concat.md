![[Dashboard Traceability.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Case nya|Case nya]]
- [[#GROUP CONCAT|GROUP CONCAT]]

## Introduction
Saya dapat project sederhana untuk membuat dashboard untuk pengecekan apakah line dan lubang sudah memberikan bumbu pada mie nya? Sederhana tapi tricky.

_Menariknya_ jadi gini... saya menyambungkan 2 table:

1. noodle_2_bumbu_transaksi_lubang
2. noodle_2_bumbu_master_shift

Untuk struktur table nya seperti ini: table: noodle_2_bumbu_transaksi lubang

<img src="Dashboard Traceability_1.png" width="350" align="left">













jadi id_transaksi di join ke noodle_2_bumbu_master_shift. jam_ke ini kita akan check jika ada maka kita berikan penjelasan.

table: noodle_2_bumbu_master_shift

<img src="Dashboard Traceability_2.png" width="350" align="left">











## Case nya
Saya mau `groupBy('noodle_2_bumbu_master_shift.line','noodle_2_bumbu_transaksi_lubang.lubang_ke')`. Group by di 2 table yg berbeda. dan otomatis kalau hasil groupby maka kita bisa ambil data paling pertama. saya maunya groupby juga, TAPI mempertahankan isi :
1. jam_ke
2. jam_pengisian
3. material_group Semalaman saya belum dapat jawaban. Sampai pagi nya baru ketemu jawabannya.

## GROUP CONCAT
Fungsi `GROUP_CONCAT` pada query yang diberikan digunakan untuk menggabungkan nilai-nilai dari kolom yang sesuai ke dalam satu string dengan delimiter tertentu.

```php
public function getData(Request $req)
    {
	$trace = DB::table('noodle_2_bumbu_transaksi_lubang')
		->join('noodle_2_bumbu_master_shift','noodle_2_bumbu_transaksi_lubang.id_transaksi','=','noodle_2_bumbu_master_shift.id')
		->select('noodle_2_bumbu_transaksi_lubang.id_transaksi',
			'noodle_2_bumbu_transaksi_lubang.tgl_produksi',
			'noodle_2_bumbu_master_shift.line',
			'noodle_2_bumbu_transaksi_lubang.lubang_ke',
			DB::raw('GROUP_CONCAT(noodle_2_bumbu_transaksi_lubang.jam_ke) as jam'),
			DB::raw('GROUP_CONCAT(noodle_2_bumbu_transaksi_lubang.jam_pengisian) as jam_pengisian'),
			DB::raw('GROUP_CONCAT(noodle_2_bumbu_transaksi_lubang.material_group) as material_group'),
			'noodle_2_bumbu_master_shift.shift',
			'noodle_2_bumbu_master_shift.tgl_pengisian'
		)
		->where('noodle_2_bumbu_master_shift.tgl_pengisian',$req->tanggal_filter)
		->where('noodle_2_bumbu_master_shift.shift', $req->shift_filter)
		->groupBy('noodle_2_bumbu_master_shift.line','noodle_2_bumbu_transaksi_lubang.lubang_ke')->get();

	return DataTables::of($trace)->make(true);
}
```

```javascript
let defaultShift = $("#shiftFilter").val();
let defaultTanggal = moment().format('YYYY-MM-DD');

$("#tanggalFilter").val(defaultTanggal);

function table(){
    $("#tableTraceability").DataTable({
        info: false,
        searching: false,
        bDestroy: true,
        ordering: false,
        paging: false,
        language: {
            emptyTable: "Tidak ada data yang tersedia."
        },
        ajax: {
            type: "GET",
            url: "/traceability/dashboard/getData",
            data: {
                shift_filter: defaultShift,
                tanggal_filter: defaultTanggal
            }
        },
        columns: [
            {
                width: '5%',
                data: 'line'
            },
            {
                width: '5%',
                data: 'lubang_ke'
            },
            {
                // noodle_2_bumbu_transaksi_lubang.jam_ke == 8
                render: function(data, type, row){
                    var jams = row.jam.split(',');
                    var hasJam8 = jams.includes('8');

                    var jamPengisians = row.jam_pengisian.split(',');
                    var jamPengisians8 = [];

                    jams.forEach(function(jam, index){
                        if(jam == '8'){
                            jamPengisians8.push(jamPengisians[index]);
                        }
                    });

                    var materialGroups = row.material_group.split(',');
                    var materialGroup8 = [];

                    jams.forEach(function(jam, index){
                        if(jam == '8'){
                            materialGroup8.push(materialGroups[index]);
                        }
                    });

                    var html = '<div class="card bg-success bg-success-o-75 p-1">';
                    jamPengisians8.forEach(function(jamPengisian, index){
                        html += `${materialGroup8[index]} - ${jamPengisian}</br>`;
                    });

                    html += '</div>';

                    if(hasJam8){
                        // Change the color into green
                        return html;
                    }else{
                        return "<span class='label label-warning w-100 bg-warning-o-75 label-inline text-dark'><i class='la la-clock text-dark'></i> Waiting</span>";
                    }
                }
            }
        ]
    });
}

$(document).on("change", "#tanggalFilter", function(){
    defaultTanggal = $(this).val();
    table();
});

$(document).on("change", "#shiftFilter", function(){
    defaultShift = $(this).val();
    table();
});

var currentInterval = setInterval(function(){
    table();
    console.log('refreshed by first interval');
}, $("#refreshTime").val() * 1000);

$(document).on("change", "#refreshTime", function(){
    clearInterval(currentInterval);
    currentInterval = setInterval(function(){
        table();
        console.log('refreshed');
    }, $(this).val() * 1000);
});

table();
```

Dari Group Concat maka akan menggabungkan semua hasil di 1 kolom. `row.jam_pengisian.split(',');` dan melakukan looping di render function. Nah disana kan ada otomatis refresh dalam 1, 5, 10,20 menit terserah pilihan kamu, gunakan `setInterval`.

`setInterval` di JavaScript digunakan untuk menjalankan sebuah fungsi atau kode secara berulang dengan interval waktu yang tetap. Fungsi ini menerima dua parameter: fungsi yang akan dijalankan dan interval waktu dalam milidetik.

Date: 19-07-2024