#Tech 
# Table of Content
- [[#Intro|Intro]]
- [[#Install|Install]]

## Intro
Rencana awal saya ingin membuat blog untuk [web pribadi](https://zyandaru.xyz/) saya mau coba 1 package ini yg bisa membuat saya mempermudah pekerjaan saya. Terimakasih banyak kepada [Mohamed Said](https://themsaid.com/). Dokumentasi Lengkap [disini](https://github.com/themsaid/wink)

## Install
- Instal package Laravel winks terlebih dahulu -> `composer require themsaid/wink`
- Install keperluan wink di laravel -> `php artisan wink:install`
- membuat symbolic link dari folder `storage/app/public` ke folder `public/storage`. Ini penting karena Wink menyimpan file media (seperti gambar) di dalam folder `storage` -> `php artisan storage:link`
- Jangan lupa migrasi -> `php artisan wink:migrate`

## Error
![[Pasted image 20250404074420.png]]

Bila terjadi error seperti ini silahkan buka file `config/wink.php` dan cari kode berikut:

```php
'database_connection' => env('WINK_DB_CONNECTION', 'wink'),
```

Ubah menjadi

```php
'database_connection' => env('DB_CONNECTION'),
```

Selengkapnya: https://welcm.uk/blog/laravel-wink-a-beginners-guide