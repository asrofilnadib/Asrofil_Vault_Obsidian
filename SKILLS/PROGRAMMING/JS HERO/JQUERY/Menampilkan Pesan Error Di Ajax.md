#Tech 
## Introduction
Kebiasaan buruk gue adalah ketika error tetep gue masukin response 201 (created). Nah disini sederhana, gue mau terapin kode error yg sesuai dengan aturan.

## Jenis Kode Error
Berikut yg sering dipakai:

1. 200 (OK) = Permintaan berhasil dan data dikirim kembali
2. 201 (Created) = Data berhasil dibuat
3. 400 (Bad Request) = Format request salah (JSON tidak valid, parameter tidak sesuai)
4. 401 (Unauthorized) = Token tidak valid atau tidak login
5. 403 (Forbidden) = Akses ditolak meskipun user login (hak akses tidak cukup)
6. 404 (Not Found) = Halaman atau endpoint tidak ditemukan
7. 405 (Method Not Allowed) = Mengakses route dengan method yang salah (POST ke route GET).|
8. 422 (Unprocessable Entity) = Validasi gagal
9. 500 (Internal Server Error) = Kesalahan di server
10. 502 (Bad Gateway) = Server menerima respons tidak valid dari upstream server

## Implementasi

```php
$validator = Validator::make($request->all(), [...]);
if ($validator->fails()) {
    return response()->json(['errors' => $validator->errors()], 422);
}
```

```javascript
error: function(xhr) {
	if (xhr.status === 422) {
		let res = JSON.parse(xhr.responseText);
		notif('error', 'Gagal', res.message);
	} else {
		notif('error', 'Error', 'Terjadi kesalahan saat menyimpan data.');
	}
}
```

parsing xhr.responseText nya jangan lupa

Date: 15-05-2025