#Tech
# Table of Content
- [[#Introduction|Introduction]]
- [[#Setting XAMPP|Setting XAMPP]]
- [[#Jalankan Server Laravel|Jalankan Server Laravel]]

## Introduction
May 2023, Semester 8 saya diminta Joki teman saya yang riset di SMK nya. SMKN 55 Jakarta, Pademangan. Saat itu saya menemani mas Rizaq untuk meminta surat riset, singkat cerita pihak sekolah meminta aplikasi tersebut untuk digunakan dalam 1 Jaringan.

Sepengetahuan saya, untuk _localhost_ hanya dapat diakses di device itu saja (yang menyalakan server). Setelah ngide di google, ternyata _ADA_ dan _BISA_. ini digunakan di kampus saya ketika absensi. Jadi intinya hanya bisa digunakan ditempat itu dan hanya di 1 jaringan yang sama.

## Setting XAMPP
Buka file **httpd.conf** di folder **C:/xampp/apache/conf**/httpd.conf

## Jalankan Server Laravel
Jika menggunakan laravel, umumnya kita menggunakan perintah berikut :

```shell
php artisan serve
```

Namun lain hal ketika kita ingin server bisa _dijalankan untuk banyak device dalam 1 jaringan_.

```shell
php artisan serve --host 192.168.1.100 --port 8001
```

Untuk mengetahui IP Address, silahkan buka cmd dan ketik ipconfig dan masukkan IPV4 Address nya...