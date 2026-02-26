#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Middleware|Middleware]]
- [[#Penggunaan https|Penggunaan https]]
- [[#Jangan lupa register di kernel.php|Jangan lupa register di kernel.php]]

## Introduction
Kelanjutan dari [[Auto Redirect Https]], ini penting ketika project lo memang diharuskan untuk menggunakan http dan tidak bisa https. Contoh: websocket nya laravel.

## Middleware
```php
<?php
namespace App\Http\Middleware;

use Closure;

class ToHttp
{
    public function handle($request, Closure $next)
    {
        if ($request->isSecure()) {
            return redirect()->to('http://' . $request->getHost() . $request->getRequestUri());
        }

        return $next($request);
    }
}

```

Penjelasan:
- `if ($request->secure())`: jika kamu menggunakan https, maka perintah didalam if akan dijalankan.
- **`$request->getHost()`**: Ini adalah metode yang mengembalikan nama host dari permintaan saat ini.

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
	'http' => \App\Http\Middleware\ToHttp::class,
];
```

Date: 26-02-2025