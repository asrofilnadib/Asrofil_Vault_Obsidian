![[Filesystem(1).png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Cara Pertama|Cara Pertama]]
- [[#Cara kedua|Cara kedua]]
- [[#Controller|Controller]]

## Introduction
Problem ketika saya mendapat project CBT(Computer Based Test) untuk SMA Yappenda. Problemnya terletak ketika upload image di localhost aman namun ketika deploy ke cpanel error! Hal yg wajar :)

Jadi saya ingin mencoba untuk mengubah 'storage' ke 'public'. Simple nya kita konfigurasi di `config/filesystems.php`.

## Cara Pertama
Cukup ubah root => `storage_path()` ke root => `public_path()`

```php
'local' => [
	'driver' => 'local',
	'root' => public_path(),
	'throw' => false,
],
```

## Cara kedua
Jika masih error lakukan cara ini.

```php
'public' => [
	'driver' => 'local',
	'root' => public_path(),
	'url' => env('APP_URL'),
	'visibility' => 'public',
	'throw' => false,
],
```

dan ubah juga defaultnya menjadi public:

```php
'default' => env('FILESYSTEM_DISK', 'local'),
```

## Controller
```php
$imageName = Carbon::parse(now())->format('dmYHis').'.'.$this->picture->getClientOriginalExtension();
$this->picture->storeAs('assets/images/users', $imageName);
```

Saya menggunakan komponen livewire untuk upload file gambar.

Date : 10-03-2024