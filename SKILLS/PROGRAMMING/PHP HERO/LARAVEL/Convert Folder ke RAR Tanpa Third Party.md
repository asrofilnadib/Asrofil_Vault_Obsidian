#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Implementasi|Implementasi]]

## Introduction
Gue dapet case dimana gue pengen 1 folder (termasuk isinya) di convert ke RAR tanpa plugin atau third party. Case ini gue dapet ketika membuat system web generator untuk temen gue (owner bisnis).

## Implementasi
```php
public function extractToRar($template,$alamat_website)
{
	$folderPath = public_path("download/{$template}");
	$rarFilePath = public_path("download/{$template}_{$alamat_website}.rar");

	if (!is_dir($folderPath)) {
		return response()->json(['error' => 'Template folder Tidak Ada.'], 404);
	}

	// di Linux
	$command = "rar a -ep1 {$rarFilePath} {$folderPath}/*";
	// di Windows
	 $command = "rar a -r -ep1 \"{$rarFilePath}\" \"{$folderPath}/*\"";
	exec($command, $output, $returnVar);
	
	if ($returnVar !== 0) {
		return response()->json(['error' => 'Gagal Ekstrak Folder RAR file.'], 500);
	}

	return response()->download($rarFilePath)->deleteFileAfterSend(true);
}
```

Penjelasan:
- `$folderPath` untuk lokasi asli foldernya.
- `$rarFilePath` untuk nama .rar nya
- `$command`: Merupakan perintah shell untuk membuat file RAR menggunakan utilitas `rar`.
    - `rar a`: Menambahkan file ke arsip RAR.
    - `-ep1`: Menghilangkan jalur relatif dari file yang dikompres (hanya menyimpan nama file tanpa struktur folder). kalo tanpa ini tetap bisa di convert ke rar tapi isinya akan struktur lengkap. Misal klo di ubuntu akan mengambil folder `var/www/html/namaproject` padahal kita hanya butuh folder `public/download/namafolder`
    - `{$rarFilePath}`: Path file RAR yang akan dibuat.
    - `{$folderPath}/*`: Semua file di dalam folder `$folderPath` akan dimasukkan ke dalam file RAR.
- `exec($command, $output, $returnVar)`:
    - `exec()`: Menjalankan perintah shell.
    - `$output`: Menyimpan output dari perintah shell (jika ada).
    - `$returnVar`: Menyimpan kode status hasil eksekusi perintah (0 berarti sukses, nilai lain berarti gagal).

Date: 20-04-2025