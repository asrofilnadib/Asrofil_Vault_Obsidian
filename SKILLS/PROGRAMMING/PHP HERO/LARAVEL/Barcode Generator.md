![[Laravel Qrcode.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Install|Install]]
- [[#Controller|Controller]]
	- [[#Controller#Koneksi ke Wifi|Koneksi ke Wifi]]
- [[#Render di Blade|Render di Blade]]

## Introduction
Hari ini saya iseng untuk implementasi salah satu package untuk keperluan qrcode. Sebelumnya saya sempat diminta untuk membuat aplikasi absen berbasis qrcode dari salah satu sekolah swasta di Jakarta Utara.

## Install
```shell
composer require simplesoftwareio/simple-qrcode "~4"
```

## Controller
```php
<?php

namespace App\Http\Controllers;

use SimpleSoftwareIO\QrCode\Facades\QrCode;

class QrCodeController extends Controller
{
    public function show()
    {
        return QrCode::generate(
            'Hello, World!', // bisa masukkan alamat website
        );
    }
}
```

### Koneksi ke Wifi
Hebatnya package ini bisa untuk koneksi ke wifi.

```php
return QrCode::size(200)->wiFi([
	'encryption' => 'WPA/WEP',
	'ssid' => 'andarutr',
	'password' => 'triadi2001',
]);
```

## Render di Blade
```php
{!! QrCode::generate('<https://google.com>') !!}
```

Referensi : [https://harrk.dev/qr-code-generator-in-laravel-10-tutorial/](https://harrk.dev/qr-code-generator-in-laravel-10-tutorial/)

Date : 15-03-2024