![[screenshot.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Installation|Installation]]
- [[#Using|Using]]

## Introduction
Berawal dari ingin membersihkan kodingan _routes_. Pada laravel dapat melihat route dengan perintah _php artisan route:list_ . Namun kurang rapih, bisa dibilang berantakan.

Tapi, bagaimana jika, seandainya saja, seseorang ingin agar aplikasinya dapat menampilkan daftar route di aplikasi yang dia punya? Tak perlu tau tujuan spesifiknya, yang penting route bisa diakses melalui peramban.

Daripada repot membuat dari awal, lebih baik menggunakan library siap pakai, ialah [Pretty Routes](https://github.com/garygreen/pretty-routes).

## Installation
```
composer require garygreen/pretty-routes
```

**config/app.php**

```php
'providers' => [
	....
	PrettyRoutes\ServiceProvider::class,
]
```

```
php artisan vendor:publish --provider="PrettyRoutes\ServiceProvider"
```

## Using
Kamu cukup akses `localhost:8000/routes`. That's it!