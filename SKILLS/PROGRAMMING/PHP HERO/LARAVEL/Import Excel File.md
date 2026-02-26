![[UploadByExcel.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Frontend nya dulu|Frontend nya dulu]]
- [[#Backend nya|Backend nya]]
- [[#Import nya|Import nya]]
	- [[#Import nya#Ringkasan:|Ringkasan:]]

## Introduction
Upload kita biasanya paling foto, nah ini case unik. kita intinya upload excel dan data tersebut kita insert ke table.

## Frontend nya dulu
```html
<button class="btn btn-sm btn-success mt-3 mb-3" onClick="btnUpload()">Upload excel</button>
```

```javascript
function btnUpload(){
    $("#judulModal").text("Upload By Excel");

    $("#contentModal").empty();

    $("#contentModal").append(`
        <div class="mt-1">
            <div class="row">
                <div class="col-lg-12">
                    <label for="">Upload excel</label><br>
                    <input type="file" class="form-control" id="excel_file" accept=".xlsx,.xls">
                </div>
                <div class="col-lg-12">
                    <button class="btn btn-success rounded-pill mt-3" onClick="uploadExcel()">Upload</button>
                </div>
            </div>
        </div>
    `);

    $("#modal").modal("show");
}

function uploadExcel(){
    let excelFile = $("#excel_file")[0].files[0];
    let formData = new FormData();
    formData.append('excel_file', excelFile);

    $.ajax({
        type: "POST",
        url: "/fl_produksi/master/no_roll_spb/upload_excel",
        data: formData,
        processData: false,
        contentType: false,
        success: function(res) {
            table();

            Swal.fire({
                icon: "success",
                title: "Berhasil",
                text: "Berhasil import excel."
            });

            $("#modal").modal("hide");
        },

        error: function(xhr) {

            let message = xhr.responseJSON.message || 'Terjadi kesalahan saat mengunggah file.';
            notifyError(message);
        }
    });
}
```

## Backend nya
```php
public function uploadExcel(Request $req)
{
	$validator = Validator::make($req->all(), [
		'excel_file' => 'required|file|mimes:xlsx,xls',
	]);

	if ($validator->fails()) {
		return response()->json(['message' => $validator->errors()->first()], 422);
	}

	if ($req->hasFile('excel_file')) {
		$file = $req->file('excel_file');
		if (!$file->isValid()) {
			return response()->json(['message' => 'File tidak valid atau rusak.'], 400);
		}

		try {
			if ($file->getClientOriginalExtension() !== 'xlsx') {
				return response()->json(['message' => 'Format file tidak valid. Hanya menerima file .xlsx.'], 400);

			Excel::import(new MasterRoll, $file);

			return response()->json(['message' => 'Data berhasil diunggah dan diproses.'], 200);
		} catch (\Exception $e) {
			\Log::error('Error during file upload: ' . $e->getMessage());

			return response()->json(['message' => 'Terjadi kesalahan saat mengimpor data.'], 500);
		}
	}

	return response()->json(['message' => 'File tidak ditemukan atau tidak valid.'], 400);
}
```

## Import nya
```php
<?php

namespace App\Imports\FLProduksi;

use App\MasterRollSpb;
use Maatwebsite\Excel\Concerns\ToModel;
use Maatwebsite\Excel\Concerns\WithHeadingRow;

class MasterRoll implements ToModel, WithHeadingRow
{
    public function model(array $row)
    {
        try {
            $existingRoll = MasterRollSpb::where('no_roll', $row['no_roll'])
                                ->where('no_spb', $row['no_spb'])
                                ->first();

            if ($existingRoll) {
                $existingRoll->update([
                    'no_roll' => $row['no_roll'],
                    'no_spb' => $row['no_spb'],
                ]);

                return null;
            }

            return new MasterRollSpb([
                'no_roll' => $row['no_roll'],
                'no_spb' => $row['no_spb'],
            ]);
        } catch (\Exception $e) {
            throw $e;
        }

        return null;
    }
}

```

1. **Namespace & Import**:
    - Kode berada dalam namespace `App\Imports\FLProduksi` dan menggunakan model `MasterRollSpb`.
    - Juga mengimpor `ToModel` dan `WithHeadingRow` dari paket `Maatwebsite\Excel` untuk menangani import Excel dengan header baris pertama sebagai kunci data.
2. **Fungsi `model(array $row)`**:
    - Fungsi ini dipanggil untuk setiap baris data Excel yang diimport.
3. **Logika Pengecekan Data**:
    - **Pencarian Data**: Kode mencari apakah ada data dengan `no_roll` dan `no_spb` yang sama di database (`MasterRollSpb`).
    - **Update Jika Ditemukan**: Jika ditemukan, data yang sudah ada akan diperbarui (`update()`), dan proses import untuk baris ini dihentikan (`return null`).
    - **Insert Jika Tidak Ada**: Jika tidak ditemukan, data baru akan dibuat dan disimpan ke tabel.
4. **Error Handling**:
    - Menggunakan blok `try-catch` untuk menangani pengecualian (error) dan membuang pesan error jika terjadi.

### Ringkasan:
- Jika baris data (`no_roll` dan `no_spb`) sudah ada di database, maka diperbarui.
- Jika tidak ada, data baru ditambahkan.
- Fungsi ini menangani impor data dari file Excel ke tabel `MasterRollSpb`.

Date: 07-10-2024