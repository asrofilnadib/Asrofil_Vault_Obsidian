![[Pasted image 20250607132538.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Frontend nya|Frontend nya]]
- [[#Backend nya|Backend nya]]

## Introduction
Jurnal ini adalah versi lanjutan dan versi sederhana dari [[Import Excel File]].

## Frontend nya
Html nya
```html
<div class="modal fade" id="importExcelModal" tabindex="-1" role="dialog" aria-labelledby="importExcelModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="importExcelModalLabel">
                    <i class="mdi mdi-file-excel"></i>
                    Import Data dari Excel
                </h5>
                <button type="button" class="btn-close" data-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <form id="import-excel-form" method="post" enctype="multipart/form-data" action="{{ route('pas-downtime.admin.master-machine.import-excel') }}" required>
                    @csrf
                    <div class="form-group">
                        <label for="excel_file">Pilih File Excel</label>
                        <input type="file" class="form-control" name="excel_file" id="excel_file" required>
                    </div>

                    <div class="d-flex justify-content-between mt-3">
                        <button class="btn btn-primary" type="submit">
                            <i class="mdi mdi-check-all"></i>
                            Import
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
```

JS nya
```javascript
$('#import-excel-form').submit(function(e) {
	e.preventDefault();

	$.ajax({
		url: $(this).attr('action'),
		type: 'POST',
		data: new FormData(this),
		contentType: false,
		cache: false,
		processData: false,
		success: function(response) {
			// kode anda
		},
		error: function(xhr, status, error) {
			Swal.fire({
				icon: 'error',
				title: 'Gagal',
				text: xhr.responseJSON.message
			});
		}
	});
});
```

## Backend nya
PHP nya
```php
public function import(Request $request)
{
	$request->validate([
		'excel_file' => 'required|mimes:xlsx,xls',
	]);

	$file = $request->file('excel_file');

	$data = Excel::toArray(null, $file);

	$data = $data[0];

	unset($data[0]);

	$data = array_map(function($item) {
		$area_name = $item[0];

		$area = MArea::where('area_name', $area_name)->first();

		if ($area == null) {
			return [
				'area_code' => '',
				'area_name' => '',
				'machine_name' => $item[1],
			];
		}

		return [
			'area_code' => $area->area_code,
			'area_name' => $area->area_name,
			'machine_name' => $item[1],
		];
	}, $data);

	$data = array_filter($data, function($item) {
		return $item['machine_name'] != null;
	});

	return response()->json([
		'data' => $data,
		'status' => 'success',
	]);
}
```
Penjelasan:
1. **Validasi Request**
    - Memastikan bahwa request memiliki file dengan nama `excel_file`.
    - File harus bertipe `xlsx` atau `xls` (format Excel).
2. **Mengambil File Excel**
    - Mengambil file dari input menggunakan `$request->file('excel_file')`.
3. **Membaca Data Excel**
    - Menggunakan package `Maatwebsite\Excel` (`Excel::toArray()`) untuk membaca seluruh isi file Excel.
    - Hasil dibaca sebagai array multidimensi, dan hanya sheet pertama (`$data[0]`) yang digunakan.
4. **Menghapus Baris Header**
    - Baris pertama (biasanya header kolom) dihapus menggunakan `unset($data[0])`.
5. **Memetakan Setiap Baris Data**
    - Setiap baris data di-loop menggunakan `array_map`.
    - Kolom pertama (`item[0]`) adalah `area_name`, dicari di tabel `MArea`.
    - Jika `area_name` tidak ditemukan:
        - Tetap mengembalikan area kosong dan `machine_name` dari Excel.
    - Jika ditemukan:
        - Tambahkan `area_code` dan `area_name` dari database, serta `machine_name` dari Excel.
6. **Filter Data Kosong**
    - Hapus semua baris yang memiliki nilai `machine_name` kosong menggunakan `array_filter`.
7. **Mengembalikan Response JSON**
    - Memberikan respons sukses berupa JSON yang berisi:
        - `data`: Array hasil pemrosesan.
        - `status`: `'success'`.

Date: 07-06-2025