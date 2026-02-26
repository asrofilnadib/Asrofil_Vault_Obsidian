#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#$fillable|$fillable]]
	- [[#$fillable#Mass Asignment Vulnerability|Mass Asignment Vulnerability]]
- [[#$guarded|$guarded]]

## Introduction
Kenapa saya membuat Pembahasan ini? Karena saya sempat mengalami 1 jam debugging ternyata salahnya dimana? saya belum menambahkan salah satu kolom di Model.

## $fillable
Di Laravel, `$fillable` adalah properti dalam model yang digunakan untuk menentukan atribut mana saja yang boleh diisi. Hal ini melindungi model dari `mass asignment vulnerability`

```php
class User extends Model
{
    protected $fillable = ['name', 'email', 'password'];
}
```

Hanya kolom yang didefinisikan di `$fillable` yang bisa diisi menggunakan metode `create()` atau `update()`. Jika kita mencoba memasukkan kolom lain yang tidak ada di `$fillable`, Laravel akan mengabaikannya untuk mencegah perubahan data yang tidak diinginkan. Biasanya bila ada default akan digunakan jika tidak ada kolom bersangkutan di `$fillable`.

### Mass Asignment Vulnerability
**Mass assignment vulnerability** adalah kerentanan keamanan yang terjadi ketika semua input dari pengguna dapat langsung digunakan untuk mengisi atribut model tanpa validasi. Kerentanan ini memungkinkan pengguna untuk memodifikasi data yang seharusnya tidak boleh mereka ubah, karena atribut yang tidak diinginkan atau sensitif bisa di-_assign_ secara massal ke model.

Contoh kerentanan:

```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "secret",
    "is_admin": true
}
```

Tanpa proteksi dari Laravel, atribut `is_admin` akan di-_assign_ secara langsung, dan pengguna biasa bisa menjadikan dirinya sendiri sebagai admin.

## $guarded
Selain `$fillable`, Laravel juga menyediakan properti `$guarded` di model untuk mengontrol _mass assignment_, tetapi dengan pendekatan yang berlawanan. Jika `$fillable` digunakan untuk menentukan kolom mana yang **boleh** di-_mass assign_, maka `$guarded` digunakan untuk menentukan kolom mana yang **tidak boleh** di-_mass assign_.

```php
class User extends Model
{
    protected $guarded = ['is_admin'];
}
```

```php
public function store(Request $request)
{
    $data = $request->only(['nama', 'umur', 'hobi']);
    $data['is_admin'] = true;
    User::create($data);

    return redirect()->back()->with('success', 'User berhasil dibuat dengan is_admin true.');
}
```

Date: 12-10-2024