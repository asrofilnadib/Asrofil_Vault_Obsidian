#Tech 
# Table of Content
- [[#Laravel Sanctum|Laravel Sanctum]]
- [[#Introduction|Introduction]]
- [[#Instalasi|Instalasi]]
- [[#Model User|Model User]]
- [[#Kernel|Kernel]]
- [[#Routing|Routing]]
- [[#Controllernya|Controllernya]]
- [[#Try & Catch|Try & Catch]]
- [[#Screenshot|Screenshot]]

## Laravel Sanctum
Laravel Sanctum adalah solusi _autentikasi_ yang dikembangkan oleh tim Laravel untuk menangani autentikasi API dengan token dan sesi berbasis cookie. Sanctum dirancang untuk menawarkan kemudahan penggunaan dan keamanan dengan cara yang ringan dan mudah diintegrasikan dalam aplikasi Laravel.

## Introduction
Saya mencoba untuk menggunakan Authentication menggunakan token dengan Laravel Sanctum. Ternyata sangat sederhana. Berikut caranya

## Instalasi
- composer require laravel/sanctum
- php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider
- php artisan migrate

## Model User
```php
<?php
namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens ,HasFactory, Notifiable;
    // ...
}
```

Tambahkan HasApiTokens milik sanctum di model User

## Kernel
```php
'api' => [
	\Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
	'throttle:api',
	\Illuminate\Routing\Middleware\SubstituteBindings::class,
],

```

Tambahkan ini di app/http/kernel.

## Routing
```php
Route::post('/auth/register', [UserController::class, 'register']);
Route::post('/auth/login', [UserController::class, 'login']);
```

Contoh Routing. Taruh di routes/api.php

## Controllernya
```php
<?php

namespace App\\\\Http\\\\Controllers\\\\Api;

use Throwable;
use App\Models\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Hash;
use Illuminate\Support\Facades\Validator;

class UserController extends Controller
{
    public function register(Request $req)
    {
        try{
            $validator = Validator::make($req->all(),[
                'name' => 'required',
                'email' => 'required|email|unique:users,email',
                'password' => 'required|min:6'
            ]);

            if($validator->fails()){
                return response()->json([
                    'status' => false,
                    'message' => 'validation errors',
                    'errors' => $validator->errors()
                ], 401);
            }

            $user = User::create([
                'name' => $req->name,
                'email' => $req->email,
                'password' => Hash::make($req->password),
            ]);

            return response()->json([
                'status' => true,
                'message' => 'Berhasil buat akun!',
                'token' => $user->createToken('Andrew Riyadi')->plainTextToken
            ], 200);
        }catch(Throwable $err){
            return response()->json([
                'status' => false,
                'message' => $err->getMessage()
            ], 500);
        }
    }

    public function login(Request $req)
    {
        try{
            $validator = Validator::make($req->all(),[
                'email' => 'required|email',
                'password' => 'required|min:6'
            ]);

            if($validator->fails()){
                return response()->json([
                    'status' => false,
                    'message' => 'validation errors',
                    'errors' => $validator->errors()
                ], 401);
            }

            if(!Auth::attempt($req->only(['email','password']))){
                return response()->json([
                    'status' => false,
                    'message' => 'Email dan password salah!'
                ], 401);
            }

            $user = User::where('email', $req->email)->first();

            return response()->json([
                'status' => true,
                'message' => 'Berhasil login!',
                'token' => $user->createToken('Andrew Riyadi')->plainTextToken
            ], 200);
        }catch(Throwable $err){
            return response()->json([
                'status' => false,
                'message' => $err->getMessage()
            ], 500);
        }
    }
}

```

Penjelasan:
- kurang lebih sama penggunaan dengan ajax, lempar-lempar json.
- menggunakan try and catch
- untuk membuat token cukup `$user->createToken('Andrew Riyadi')->plainTextToken`

## Try & Catch
`try` dan `catch` adalah blok kode dalam banyak bahasa pemrograman yang digunakan untuk menangani **eksepsi** atau **kesalahan** yang mungkin terjadi selama eksekusi program. Eksepsi adalah kondisi tidak normal yang terjadi selama proses eksekusi program, seperti kesalahan dalam file input/output, pembagian dengan nol, atau kegagalan jaringan.

## Screenshot
![[Laravel Sanctum Auth (1).png]]
![[Laravel Sanctum Auth (2).png]]

Referensi: [https://www.youtube.com/watch?v=ajUST-jUMeg](https://www.youtube.com/watch?v=ajUST-jUMeg)

Date: 08-08-2024