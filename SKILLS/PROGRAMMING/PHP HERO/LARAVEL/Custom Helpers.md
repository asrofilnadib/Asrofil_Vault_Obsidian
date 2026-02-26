#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Membuat Helpers|Membuat Helpers]]
- [[#Register in Service Provider|Register in Service Provider]]
- [[#Make It Global!|Make It Global!]]
- [[#The Controllers|The Controllers]]
- [[#Shortcut|Shortcut]]

## Introduction
Laravel hadir dengan berbagai macam Helpers, seperti: penggunaan DB, session, dan masih banyak sebagainya. Pada dasarnya, helpers dapat digunakan darimanapun atau biasa dikenal _Global Function_. Namun bagaimana jika helpers yg sesuai dengan kebutuhan anda tidak tersedia?

Solusinya adalah kamu bisa membuat helpers sendiri sesuai kebutuhan kamu. Sebagai contoh saya ingin _track aktifitas_ semua user. Entah itu dia sedang login hingga logout.

## Membuat Helpers
Silahkan buat folder _Helpers_ dalam folder _App_ dengan nama file _RecordActivity.php_. Ketik code berikut :

```php
<?php
namespace App\Helpers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;

class RecordActivity {
    public static function track($do)
    {
        \DB::table('activity')->insert([
            'id_user' => Auth::user()->id,
            'activity' => $do,
            'created_at' => now()
        ]);
    }
}
```

Sebelum dapat digunakan secara global, kita harus menempatkan nya kedalam service provider.

- _php artisan make:provider UserServiceProvider_

## Register in Service Provider
Langkah selanjutnya, register helpers kamu di service provider -> _App\Providers\UserServiceProvider.php_.

```php
public function register()
{
	require_once app_path() . '/Helpers/RecordActivity.php';
}
```

## Make It Global!
buka folder _config/app.php_ dan masukkan UserServiceProvider kamu kedalam :
1. Providers
2. Aliases

```php
'providers' => [
	.........................
	App\Providers\UserServiceProvider::class,
],

'aliases' => [
	'Record' => App\Helpers\RecordActivity::class,
]
```

## The Controllers
```php
use App\Helpers\RecordActivity;

// Record to Activity
RecordActivity::track('Login');
```

## Shortcut
Jika ingin menggunakan cara lain, kamu dapat langsung mengakses aliases dengan menambahkan _backslash ('\'')_

```php
\Record::track('Login');
```