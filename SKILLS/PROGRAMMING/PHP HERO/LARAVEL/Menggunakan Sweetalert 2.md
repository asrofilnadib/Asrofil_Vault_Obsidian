#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Installing|Installing]]
- [[#Register The Service Provider|Register The Service Provider]]
- [[#Configuration|Configuration]]
- [[#Middleware|Middleware]]
- [[#Using Sweetalert 2|Using Sweetalert 2]]
- [[#Turn Off Error Message|Turn Off Error Message]]

## Introduction
A BEAUTIFUL, RESPONSIVE, CUSTOMIZABLE, ACCESSIBLE (WAI-ARIA) REPLACEMENT FOR JAVASCRIPT'S POPUP BOXES FOR LARAVEL. Sweetalert adalah popup boxes cantik yang biasa digunakan oleh developer untuk memberikan informasi seperti data berhasil ditambahkan. Dokumentasi lebih lanjut silahkan klik [disini](https://realrashid.github.io/sweet-alert/)

## Installing
```
composer require realrashid/sweet-alert
```

## Register The Service Provider
Pergi ke config/app.php, tambahkan kode berikut

```php
'providers' => [
	/* * Package Service Providers... */
	RealRashid\SweetAlert\SweetAlertServiceProvider::class,
],

'aliases' => [
	.........
	'Alert' => RealRashid\SweetAlert\Facades\Alert::class,
],
```

## Configuration
Silahkan tambahkan kode berikut pada footer kamu

```html
@include('sweetalert::alert')
```

Dan jalankan perintah berikut untuk publish package asset milik sweetalert2

```
php artisan sweetalert:publish
```

## Middleware
Pergi ke `App\Http\kernel.php` tambahkan kode berikut dalam array **$middlewareGroups**

```php
\RealRashid\SweetAlert\ToSweetAlert::class,
```

## Using Sweetalert 2

```php
return redirect('tasks')->withSuccess('Task Created Successfully!');

// atau

return redirect('tasks')->withToastSuccess('Task Created Successfully!');
```

## Turn Off Error Message

Hal seperti ini dapat mengganggu user, untuk mematikan notifikasi error nya, silahkan tambahkan kode berikut di file `.env`

```php
SWEET_ALERT_MIDDLEWARE_AUTO_CLOSE=true
SWEET_ALERT_MIDDLEWARE_TOAST_POSITION='top-end'
SWEET_ALERT_MIDDLEWARE_TOAST_CLOSE_BUTTON=true
SWEET_ALERT_MIDDLEWARE_ALERT_CLOSE_TIME=3000
SWEET_ALERT_AUTO_DISPLAY_ERROR_MESSAGES=false
```

> Thank's to RealRashid!