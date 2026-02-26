![[Pasted image 20250523132615.png]]
#Tech 
## Introduction
Jadi ada user minta ke gue gini "Mas bisa ngga itu judul nya kan ada audit halal internal dan kriteria SJPH. bisa ngga dibuatkan export yg berbeda?" coy makin kesini makin aneh requestan user. Tapi oke bakal gue turutin karena penasaran juga ogut.

## Implementasi nya
```javascript
$("#tableDetail").DataTable({
	paging: false,
	bDestroy: true,
	ordering: false,
	serverSide: true,
	dom: 'Bfrtip', 
	buttons: [
		{
			extend: 'pdfHtml5',
			text: 'Export All',
			exportOptions: {
				columns: ':not(:last-child)'
			},
			customize: function (doc) {
				doc.content.splice(0, 0, {
					text: "Data Checklist Detail",
					style: {
						bold: true,
						fontSize: 16,
						alignment: 'center'
					},
					margin: [0, 0, 0, 10]
				});
			}
		},
		{
			extend: 'pdfHtml5',
			text: 'Export Halal Internal',
			className: 'btn btn-warning',
			exportOptions: {
				modifier: {
					page: 'all'
				},
				customizeData: function(data) {
					data.body = data.body.filter(function(row) {
						return row[3] === 'Audit Halal Internal';
					});
				}
			},
			customize: function (doc) {
				doc.content.splice(0, 0, {
					text: "Laporan Audit Halal Internal",
					style: {
						bold: true,
						fontSize: 16,
						alignment: 'center'
					},
					margin: [0, 0, 0, 10]
				});
			}
		},
		{
			extend: 'pdfHtml5',
			text: 'Export Kriteria SJPH',
			className: 'btn btn-info',
			exportOptions: {
				modifier: {
					page: 'all'
				},
				customizeData: function(data) {
					data.body = data.body.filter(function(row) {
						return row[3] === 'Audit Kriteria SJPH';
					});
				}
			},
			customize: function (doc) {
				doc.content.splice(0, 0, {
					text: "Laporan Audit Kriteria SJPH",
					style: {
						bold: true,
						fontSize: 16,
						alignment: 'center'
					},
					margin: [0, 0, 0, 10]
				});
			}
		}
	],
	ajax: {
		type: "GET",
		url: "/cpar-audit/dcc/master-data/checklist/getDataChecklistDetail",
		data: {
			tahun, iso, dept
		}
	},
	columns: [
		{ 
			data: null,
			render: function(data, type, row) {
				return `<input type="checkbox" class="row-delete" value="${row.id}" />`;
			}
		},
		{ 
			data: "created_at",
			render: function(data, type, row){
				return moment(data).format("YYYY")
			}
		},
		{ data: "iso" },
		{ data: "klausul" },
		{ data: "judul" },
		{ data: "pertanyaan" },
		{ data: "dept" },
		{ 
			render: function(data, type, row){
				return `
				<center>
					<button class="btn btn-sm btn-success" onClick="updateModal('${row.id}', '${row.iso}', '${row.klausul}', '${row.dept}', '${row.judul}', '${row.pertanyaan}')"><i class="fas fa-edit"></i></button>
					<button class="btn btn-sm btn-danger" onClick="remove(${row.id})"><i class="fas fa-trash"></i></button>
				</center>
				`;
			}
		}
	]
});
```
Penjelasan:
- `className: 'btn btn-warning'`: Warna tombol kuning
- `modifier.page: 'all'`: Ekspor semua halaman tabel, bukan hanya halaman yang terlihat.
- `customizeData(data)`: Filter baris sesuai kolom ke-4 bernilai`"Audit Halal Internal"`
- `row[3]`: Merujuk ke kolom ke-4 yaitu`"Judul"`
- `customize()`: Sama seperti tombol pertama, tambahkan judul laporan di PDF.

Date: 24/5/25