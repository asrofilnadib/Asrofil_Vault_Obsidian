![[Pasted image 20250507210912.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Requirement|Requirement]]
- [[#Analogi|Analogi]]
- [[#Load Semua|Load Semua]]
- [[#Implementasi + Fitur Searching|Implementasi + Fitur Searching]]

## Introduction
Gue sempet ketemu case dimana ngeload data bisa 5 menit. User ngeluh ke gue "Pak ini saya refresh kok bisa sampe 5 menit-an ya?". Disinilah saat nya kita load data menggunakan Server Side.
**Server-Side Rendering (SSR)** adalah cara untuk **menampilkan halaman web di server** , sebelum dikirim ke browser pengguna.

## Requirement
- Install Yajra v.9-an
- Laravel 7

## Analogi
- **Client-Side Rendering (CSR)** = Kamu pesan bahan-bahannya, lalu masak sendiri di rumah. Butuh waktu.
- **Server-Side Rendering (SSR)** = Makanannya sudah dimasak di restoran, tinggal kamu terima dan makan langsung

## Load Semua
![[Pasted image 20250507205850.png]]
Jeleknya client side rendering itu adalah dia load semua data di browser di awal. kalo data lo lebih dari 10 ribu gimana? Belum lagi kalo ada relation ke table lain!

![[Pasted image 20250507210036.png]]
Nah kalo pakai server side rendering di awal dia load 10 dulu (angka default Datatable).

## Implementasi + Fitur Searching

```html
<div class="form-group">
	<label for="uomFilter">Filter Berdasarkan UOM:</label>
	<select id="uomFilter" class="form-control">
		<option value="">Semua</option>
		@foreach($uoms as $u)
		<option value="{{ $u }}">{{ $u }}</option>
		@endforeach
	</select>
</div>

<table class="table table-hover" id="tableKonversi">
	<thead>
		<tr>
			<th>MID</th>
			<th>BARANG</th>
			<th>UOM</th>
			<th>STD QTY</th>
			<th><center>ACTION</center></th>
		</tr>
	</thead>
</table>
```

Disini kita lagi buat table dan filter UOM (Unit Of Measure).

```javascript
$(document).ready(function () {
    let table = $("#tableKonversi").DataTable({
        serverSide: true,
        ajax: {
            type: "GET",
            url: "/tms/master/konversi-palet/getData",
            data: function (d) {
                d.uom = $('#uomFilter').val(); 
            }
        },
        columns: [
            { data: "mid" },
            { data: "nama_barang" },
            { data: "uom" },
            { data: "std_qty_pal" },
            {
                render: function (data, type, row) {
                    return `
                    <center>
                        <button class="btn btn-icon btn-sm btn-success btn-edit" data-id="${row.id}"><i class="fas fa-pen"></i></button>
                    </center>
                    `;
                }
            }
        ]
    });

    $('#uomFilter').on('change', function () {
        table.ajax.reload(); 
    });
});
```

Penjelasan: 
- `serverSide: true` = menyatakan bahwa kita render dari sisi server.
- `d.uom = $('#uomFilter').val();` = kita mengambil hasil select dari filter uom.
- `table.ajax.reload();` = kita melakukan reload bila ada perubahan.

```php
use Yajra\Datatables\Datatables;

public function getData(Request $req)
{
	$uom = $req->get('uom'); 

	$query = TmsMasterKonversiPalet::query();

	if ($uom) {
		$query->where('uom', $uom); 
	}

	return Datatables::of($query)
		->editColumn('std_qty_pal', function ($row) {
			return number_format($row->std_qty_pal, 0, ',', '.');
		})
		->make(true);
}
```

Penjelasan: 
- `$req->get('uom');` = kita tangkap hasil select filter uom.
- `if ($uom)` = jika kita melakukan pemilihan uom maka kode didalamnya dijalankan.
- `editColumn` = kolom sudah ada di frontend tapi mau di edit di sisi server.

Date: 07-05-2025