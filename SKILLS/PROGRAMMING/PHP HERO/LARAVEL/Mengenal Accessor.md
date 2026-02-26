#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Accessor Laravel|Accessor Laravel]]
- [[#Implementasi nya|Implementasi nya]]
	- [[#Implementasi nya#$casts|$casts]]

## Introduction
Case ini jokian temen saya, ada manipulasi angka menjadi format rupiah. bagaimana jika blade yg berkaitan dengan format rupiah lebih dari 10? pasti akan ribet dan mengulang-ulang. Disini saya baru kenal **"Accessor Laravel"** Apa itu?

## Accessor Laravel
Accessor memungkinkan kamu untuk **mengubah atau memodifikasi** data setelah diambil dari basis data, tapi sebelum ditampilkan ke pengguna. Misalnya, jika kamu ingin mengubah format data tertentu saat ditampilkan.

## Implementasi nya
Model Material

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Casts\Attribute;
use Illuminate\Database\Eloquent\Factories\HasFactory;

class Material extends Model
{
    use HasFactory;
    protected $guarded = [];

    protected $casts = [
        'harga' => 'integer',
    ];

    protected function harga(): Attribute
    {
        return Attribute::make(
            get: fn ($value) => number_format($value, 0, ',', '.')
        );
    }
}
```

Penjelasan:
- **Accessor** `harga()` menggunakan `Attribute::make()` untuk mendefinisikan cara memformat data ketika diambil (`get:`).
- Fungsi `number_format($value, 0, ',', '.')` digunakan untuk memformat angka dalam format Rupiah (tanpa desimal, menggunakan titik sebagai pemisah ribuan, dan koma sebagai pemisah desimal).
- Setiap kali Anda memanggil properti `harga` di Blade (atau di mana pun), hasilnya akan otomatis diformat.
- Di blade `{{ $material->harga }}`. Lebih simple bukan??

### $casts
`$casts` mempermudah kita dalam menjaga _konsistensi tipe data_ di aplikasi, terutama saat berinteraksi dengan database. Misalnya, jika kita ingin atribut `harga` selalu diperlakukan sebagai integer meskipun nilainya diambil dari database dalam bentuk string, `$casts` akan melakukannya secara otomatis.  

Baca: [[Define Tipe Data Kolom di Model]]

Date: 11-09-2024