![[Pasted image 20250728143705.png]]
#Tech 
## Introduction
Lanjutan dari [[Clean Code Dengan Membuat Service & Best Practice PenggunaanTelegram]], kali ini gue bakal bahas cara kirim foto ke telegram melalui Laravel. Sebenernya sangat mudah ya teman-teman. Lesgo...
## Telegram
```php
public function sendPhoto($dept, $photoPath, $caption = '')
{
	$telegram = DB::table('tms_master_telegram')->where('dept', $dept)->first();
	
	if($telegram){
		$token = $telegram->token;
		$chat_id = $telegram->chat_id;
		$url = "https://api.telegram.org/bot{$token}/sendPhoto";

		$mime = mime_content_type($photoPath);
		
		$allowedMimes = ['image/jpeg', 'image/jpg', 'image/png', 'image/webp'];
		if (!in_array($mime, $allowedMimes)) {
			Log::warning("File bukan gambar: " . $mime);
			return false;
		}

		$filename = basename($photoPath);
		$cFile = new \CURLFile($photoPath, $mime, $filename);

		$postFields = [
			'chat_id' => $chat_id,
			'photo' => $cFile,
			'caption' => $caption,
			'parse_mode' => 'Markdown'
		];

		$ch = curl_init();
		curl_setopt_array($ch, [
			CURLOPT_URL => $url,
			CURLOPT_POST => true,
			CURLOPT_POSTFIELDS => $postFields,
			CURLOPT_HTTPHEADER => ["Content-Type: multipart/form-data"],
			CURLOPT_RETURNTRANSFER => true,
			CURLOPT_SSL_VERIFYPEER => false, 
		]);

		$result = curl_exec($ch);

		if (curl_errno($ch)) {
			Log::error("Telegram Send Photo Error: " . curl_error($ch));
		}

		curl_close($ch);

		return $result;
	}else{
		$this->sendToAndaru();
	}
}
```
Penjelasan:
1. **`mime_content_type`**: Fungsi PHP yang mendeteksi tipe MIME file berdasarkan konten file (misal: image/jpeg, image/png)
2. **`allowedMime`**: Array yang berisi daftar tipe file yang diizinkan untuk memastikan hanya gambar yang diproses
3. **`basename`**: Fungsi PHP yang mengambil nama file saja dari path lengkap (misal: "/path/to/image.jpg" menjadi "image.jpg")
4. **`CURLFile`**: Class PHP yang digunakan untuk mengirim file melalui cURL dengan tipe MIME dan nama file yang benar
5. **`curl_init`**: Fungsi PHP yang menginisialisasi sesi cURL untuk melakukan request HTTP
6. **`curl_setopt_array`**: Fungsi yang mengatur banyak opsi cURL sekaligus menggunakan array untuk konfigurasi request
7. **`CURLOPT_URL`**: Opsi cURL yang menentukan URL tujuan request (dalam hal ini URL API Telegram)
8. **`CURLOPT_POST`**: Opsi cURL yang mengatur request menjadi metode POST untuk mengirim data
9. **`CURLOPT_POSTFIELDS`**: Opsi cURL yang berisi data yang akan dikirim dalam body request (file foto dan caption)
10. **`CURLOPT_HTTPHEADER`**: Opsi cURL yang menentukan header HTTP yang dikirim (Content-Type: multipart/form-data untuk upload file)
11. **`CURLOPT_RETURNTRANSFER`**: Opsi cURL yang membuat fungsi mengembalikan response sebagai string daripada menampilkannya langsung
12. **`CURLOPT_SSL_VERIFYPEER`**: Opsi cURL yang menonaktifkan verifikasi SSL certificate (false = tidak diverifikasi)
13. **`curl_exec`**: Fungsi yang mengeksekusi request cURL yang telah dikonfigurasi dan mengembalikan response
14. **`curl_errno`**: Fungsi yang memeriksa apakah terjadi error selama eksekusi cURL (mengembalikan kode error jika ada)
15. **`curl_close`**: Fungsi yang menutup dan membersihkan sesi cURL yang telah selesai digunakan.
## Laravel
```php
if ($req->hasFile('foto_breakdown')) {
	$file = $req->file('foto_breakdown');
	$originalName = $file->getClientOriginalName();
	$filename = $originalName;

	if ($filename) {
		$fotoPath = storage_path('app/public/tms/foto_breakdown/' . $filename);

		if (file_exists($fotoPath)) {
			$caption = "Contoh Caption;
			$this->telegram->sendPhoto('ALL', $fotoPath, $caption);
		}
	}

	$file->storeAs('public/tms/foto_breakdown', $originalName);
} else {
	$filename = null; 
}
```

Date: 28-07-2025
