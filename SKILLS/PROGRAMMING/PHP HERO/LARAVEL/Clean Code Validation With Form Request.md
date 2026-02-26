#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Membuat Form Request|Membuat Form Request]]
- [[#Controller Form Request|Controller Form Request]]
- [[#Call in controller|Call in controller]]

## Introduction
Melakukan validasi untuk 1 atau 3 input mungkin ngga masalah buat kamu. Bagaimana jika 10 input dan digunakan oleh banyak halaman? Jelas tidak efektif!

## Membuat Form Request
```
php artisan make:request LoginRequest
```

maka file akan berlokasi di `App\Http\Requests\LoginRequest.php`

## Controller Form Request
```php
class LoginRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [
            'email' => ['required', 'string', 'email'],
            'password' => ['required', 'string'],
        ];
    }
}
```

## Call in controller
```php
public function login(LoginRequest $req)
{
	$req->validated();
	.....................
}
```