#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Install|Install]]

## Introduction
UUID(_Universally Unique Identifier_) adalah identifier 128-bit yang secara unik mengidentifikasi sesuatu. UUID sering digunakan dalam perangkat lunak untuk memberikan cara yang unik dan sangat tidak mungkin bentrok dalam mengidentifikasi data.

UUID (Universally Unique Identifier) memiliki beberapa kegunaan dalam konteks keamanan:
1. **Pengidentifikasi Unik**: UUID digunakan sebagai pengidentifikasi unik untuk entitas dalam sistem, seperti pengguna, sesi, atau token otentikasi. Penggunaan UUID memastikan bahwa tidak ada kemungkinan bentrokan antara identifikasi, sehingga mengurangi risiko kesalahan akses atau manipulasi data.
2. **Link Rahasia**: UUID dapat digunakan untuk menghasilkan _link rahasia yang sulit ditebak_. Misalnya, ketika mengirim email verifikasi atau reset password, UUID dapat digunakan sebagai bagian dari link untuk mengidentifikasi pengguna dan tindakan yang diinginkan, sambil mempertahankan keamanan dengan menyediakan token yang sulit ditebak.

## Install
Kita akan menginstall package dari ramsey.

```
composer require ramsey/uuid
```

Contoh penerapan uuid di table users migration

```php
Schema::create('users',
function (Blueprint $table) {
	$table->uuid('id')->primary();
	$table->string('name');
	$table->string('email')->unique();
	$table->timestamps();
});
```

Untuk seeder seperti berikut :

```php
$uuid = Uuid::uuid4();

DB::table('users')->insert([
	'id' => $uuid,
	'name' => 'John Doe',
	'email' => 'john@example.com'
]);
```

Jangan lupa load class nya!

```php
use Ramsey\Uuid\Uuid;
```

Date : 22-02-2024