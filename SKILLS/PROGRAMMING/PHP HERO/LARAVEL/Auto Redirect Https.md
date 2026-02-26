#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Middleware|Middleware]]
- [[#Penggunaan https|Penggunaan https]]
- [[#Jangan lupa register di kernel.php|Jangan lupa register di kernel.php]]

## Introduction
iseng-iseng ngulik systemnya pt.pas, ada beberapa yg unik. yaitu penggunaan (wajib) https seperti untuk webcam, scanqr, dll (yg berkaitan dengan pengaksesan kamera laptop ataupun hp). Saya menggunakan cara ini karena saya pakai Laragon. Laragon itu bisa setting ssl sendiri.

## Middleware
```php
<?php
namespace App\Http\Middleware;

use Closure;

class ToHttps
{
    public function handle($request, Closure $next)
    {
        if (!$request->secure()) {
            return redirect()->secure($request->getRequestUri());
        }

        return $next($request);
    }
}
```

Penjelasan:
- `if (!$request->secure())`: jika kamu tidak menggunakan https, maka perintah didalam if akan dijalankan.
- `return redirect()->secure($request->getRequestUri());`: middleware akan mengalihkan pengguna ke URL yang sama namun dengan skema HTTPS. `$request->getRequestUri()` mendapatkan URI dari permintaan saat ini, dan `redirect()->secure()` membuat pengalihan ke URL yang aman (HTTPS).

## Penggunaan https
```php
Route::middleware('https')->group(function(){
    Route::get('/', [ReportController::class, 'index']);
});
```

## Jangan lupa register di kernel.php
```php
protected $middlewareAliases = [
	// ...
	'https' => \App\Http\Middleware\ToHttps::class,
];
```

Date: 07-08-2024