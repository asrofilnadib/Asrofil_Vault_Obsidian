#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Contoh table nya|Contoh table nya]]
- [[#Migrations nya|Migrations nya]]
- [[#Modelsnya|Modelsnya]]
- [[#Controllernya|Controllernya]]
	- [[#Controllernya#Penjelasan Penggunaan With|Penjelasan Penggunaan With]]
- [[#FE nya|FE nya]]

## Introduction
Jurnal ini lanjutan dari [[Jangan Lupa Pakai Foreign Key]]. Project ini adalah pesanan joki dari teman saya yg bekerja di PT. Sindo Jaya Logistik. Sebelumnya saya ada kendala untuk penggunaan model relation di datatable. Kali ini saya sudah dapat solusinya.

## Contoh table nya

<img src="SJL Case Relation table.png" width="450" align="left">















Intinya setiap materials memiliki category nya masing masing.

## Migrations nya
categories

```php
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('material_categories', function (Blueprint $table) {
            $table->id();
            $table->string('title', 128);
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('materials_category');
    }
};
```

materials

```php
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('materials', function (Blueprint $table) {
            $table->id();
            $table->string('title', 128);
            $table->text('note')->null();
            $table->unsignedBigInteger('category_id');
            $table->integer('stock');
            $table->decimal('harga', 15, 2);
            $table->timestamps();

            $table->foreign('category_id')->references('id')->on('material_categories')->onDelete('cascade');
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('materials');
    }
};
```

## Modelsnya
materials

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Material extends Model
{
    use HasFactory;

    protected $guarded = [];

    public function category()
    {
        return $this->belongsTo(MaterialCategory::class, 'category_id');
    }
}
```

Note: jika ada fk nya gunakan _belongsTo_

categories

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class MaterialCategory extends Model
{
    use HasFactory;

    protected $table = 'material_categories';
    protected $guarded = [];
    public $timestamps = false;

    public function material()
    {
        return $this->hasMany(Material::class, 'category_id');
    }
}
```

Note: penamaan relasi seperti `category_id` disamakan. mau yg memiliki fk atau tidak.

## Controllernya
```php
public function getDataMaterial()
{
	$material = Material::with('category')->get(); //category itu nama function nya
	return response()->json(['data' => $material]);
}
```

### Penjelasan Penggunaan With
Di Eloquent, metode `with` digunakan untuk memuat relasi model yang terkait dengan model utama dalam satu query, sebuah teknik yang dikenal sebagai _eager loading_. Ini memungkinkan kamu untuk menghindari _N+1 query problem_, di mana Eloquent akan membuat query tambahan untuk setiap relasi saat mengakses data terkait.

Mengapa Menggunakan with?
1. **Mengurangi Jumlah Query:** Tanpa `with`, Eloquent akan melakukan query terpisah untuk setiap relasi saat data diakses, yang dapat menyebabkan banyak query ke database. Dengan `with`, Eloquent memuat semua data terkait dalam satu query utama.
2. **Meningkatkan Kinerja:** Dengan memuat relasi yang dibutuhkan dalam satu query, aplikasi kamu akan lebih cepat dan efisien dalam mengakses data.
3. **Sederhanakan Kode:** Menggunakan `with` membuat kode lebih bersih dan lebih mudah dipahami karena relasi dimuat bersama dengan model utama.

## FE nya
```javascript
function getData(){
    $("#table").DataTable({
        // ...
        columns: [
            { data: 'picture' },
            { data: 'title' },
            { data: 'category.title' },
            { data: 'stock' },
            { data: 'harga' },
            { data: 'note' }
        ]
    });
}
```

`category.title` untuk memanggil kolom title di table lain.

Date: 08-09-2024