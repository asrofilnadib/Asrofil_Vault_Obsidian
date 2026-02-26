#Tech 
## Introduction
di Laravel digunakan untuk **menghapus otomatis data terkait** di tabel lain saat data induknya dihapus. Ini adalah fitur dari **foreign key constraint** di database.

## Tujuan Utama:
- Menjaga **integritas data**.
- Menghindari **data "zombie"** (data anak yang tidak punya induk lagi).
- Mengotomatiskan pembersihan data tanpa perlu kode manual di aplikasi.

## Catatan Penting:
- Fitur ini dijalankan **oleh database**, bukan oleh Laravel/Eloquent (kecuali kamu menggunakan Eloquent relationship dengan `->onDelete('cascade')` di migration).
- Pastikan kamu **benar-benar ingin menghapus data terkait**, karena ini bersifat permanen.
- Alternatif lain: `onDelete('set null')` (jika kolom nullable), yang akan mengosongkan foreign key alih-alih menghapus baris.

```php
$table->foreign('user_id')
->references('id')->on('users')
->onDelete('cascade');
```

Date: 24-10-2025