#Tech 
## Introduction
Ada case dimana gue mau buat user itu langsung lihat pdf yg tertaut di tombol. namun kendala nya adalah tidak semua komputer pt itu browsernya tidak support pdf reader sehingga terjadi error, dan lain-lain apalagi kalau default file open pdf nya ke browser. To the point, buat dia download. mau dia pakai pdf reader apapun yg penting download dulu.

## Implementasi
```html
<a href="/dokumen/laporan.pdf" download>Unduh Laporan PDF</a>
```

Penjelasan:
- Atribut `download` **berfungsi penuh hanya untuk file dari domain yang sama** (same-origin policy). Jika file dari domain lain, browser **mungkin tetap membuka file**, bukan mengunduhnya.
- Jika `download` digunakan tanpa nilai (misal: `download` saja), maka nama file akan mengikuti nama file aslinya.
- Jika `download="nama-baru.pdf"`, maka file akan diunduh dengan nama `nama-baru.pdf`.

Date: 06-10-2025