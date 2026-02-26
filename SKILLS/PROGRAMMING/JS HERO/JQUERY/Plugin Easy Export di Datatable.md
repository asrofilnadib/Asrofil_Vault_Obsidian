![[Easy Export Datatable.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Implementasi|Implementasi]]

## Introduction
Di datatable terdapat fitur menarik yaitu export excel.

## Implementasi
```javascript
$("#table").DataTable({
	// searching: false,
	// paging: false,
	info: false,
	ordering: false,
	bDestroy: true,
	dom: "<'row'<'col-sm-6 text-left'f><'col-sm-6 text-right'B>>\\\\
	<'row'<'col-sm-12'tr>>\\\\
	<'row'<'col-sm-12 col-md-5'i><'col-sm-12 col-md-7 dataTables_pager'lp>>",
	buttons: [
		{
			extend: 'excel',
			messageTop: 'Laporan Master Material Noodle 1 | Variant : (' + variant + ') | Jenis Material : ' + jenis_material,
			messageBottom: "Master Material Noodle 1",
			filename: 'Laporan Master Material Noodle 1 Variant ' + variant + ' Jenis Material ' + jenis_material,
			exportOptions: {
				columns: ':not(:last-child)'
			}
		},
	],
	ajax: {
		type: "GET",
		url: "/noodle_1/material/master/getData",
		data: {
			search, variant, jenis_material
		}
	},
	columns: [
		{ data: 'mid' },
		{ data: 'desc' },
		{ data: 'jenis_material' },
		{ data: 'variant_name' },
		{ data: 'uom' },
		{ data: 'convertion_pallet' },
		{ data: 'keterangan' },
		{
			data: 'status',
			render: function(data, type, row){
				return `
					${data == 1 ? `<span class="badge bg-primary text-white">active</span>` : `<span class="badge bg-dark text-white">non-active</span>` }
				`;
			}
		},
		{
			data: null,
			width: '10%',
			render: function(data, type, row){
				return `
				<button class="btn btn-sm btn-icon btn-success" onClick="showModalEdit(${row.id})"><i class="fas fa-edit"></i></button>`;
			}
		}
	]
});
```

Penjelasan:
- **'row'**: Mengatur elemen-elemen tabel dalam grid berbasis Bootstrap.
- **<'col-sm-6 text-left'f>**: Pada bagian kiri atas, terdapat form pencarian/filter (`f` = filter).
- **<'col-sm-6 text-right'B>**: Pada bagian kanan atas, terdapat tombol-tombol (`B` = Buttons). Dalam kasus ini, tombol untuk ekspor ke Excel.
- **<'col-sm-12'tr>**: Baris berikutnya menampilkan tabel (`tr` = table rows).
- **<'col-sm-12 col-md-5'i>**: Pada bagian bawah kiri, menampilkan informasi tentang jumlah baris yang sedang ditampilkan (`i` = info).
- **<'col-sm-12 col-md-7 dataTables_pager'lp>**: Pada bagian bawah kanan, terdapat paginasi tabel (`l` = length changing, `p` = pagination).
- **`extend: 'excel'`**: Tombol ini memungkinkan ekspor data tabel ke file Excel.
- **`messageTop`**: Menambahkan pesan di bagian atas laporan Excel. Dalam hal ini, pesan terdiri dari:
    - `'Laporan Master Material Noodle 1 | Variant : (' + variant + ') | Jenis Material : ' + jenis_material`: Menggunakan variabel `variant` dan `jenis_material` untuk membuat teks dinamis yang menunjukkan varian dan jenis material.
- **`messageBottom`**: Pesan yang ditampilkan di bagian bawah file Excel. Dalam hal ini: `'Master Material Noodle 1'`.
- **`filename`**: Nama file Excel yang akan diunduh. Nama file dibuat dinamis berdasarkan variabel `variant` dan `jenis_material`.
    - `'Laporan Master Material Noodle 1 Variant ' + variant + ' Jenis Material ' + jenis_material`
- **`exportOptions`**: Mengatur opsi ekspor, khususnya menentukan kolom mana saja yang diekspor. Dalam hal ini:
    - **`columns: ':not(:last-child)'`**: Mengekspor semua kolom kecuali kolom terakhir (biasanya kolom yang berisi tombol aksi, seperti edit atau delete).

## Lebih Rapih

```html
dom: "<'row'<'col-sm-6 text-left'B><'col-sm-6 text-right'f>>" +
	 "<'row'<'col-sm-12'tr>>" +
	 "<'row'<'col-sm-12 col-md-5'i><'col-sm-12 col-md-7'p>>",
buttons: [
	{
		extend: 'excel',
		messageTop: 'Dashboard Downtime System',
		filename: 'Dashboard Downtime System'
	},
],
```

Hasilnya akan seperti ini:
![[Pasted image 20250409102500.png]]

Date: 12-10-2024