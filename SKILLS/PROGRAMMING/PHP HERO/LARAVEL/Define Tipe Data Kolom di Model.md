![[Duh casts.png]]
#Tech 
# Table of Content
- [[#Perhatikan $casts di Model|Perhatikan $casts di Model]]

## Perhatikan $casts di Model
Case nya saya ingin looping gambar tapi kok ngga bisa? di local oke di server tidak tampil. Ngga ada salahnya coba check tipe data `$casts` kita. Jadi `casts` adalah sebuah fitur yang memungkinkan kita untuk secara eksplicit _mendefinisikan tipe data tertentu_ untuk _atribut model_ yang disimpan dalam database kita. Fitur ini sangat berguna karena memungkinkan kita untuk mengubah format data _dari dan ke_ database _tanpa harus melakukan transformasi manual_ setiap kali kita mengakses atau _menyimpan_ data model.

Contoh

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class BaCoproduct extends Model
{
    protected $table = 'ba_co_products';
    protected $guarded = [];

    protected $casts = [
        'cc' => 'array',
        'to' => 'array',
    ];

    public function signature()
    {
    	return $this->hasMany('App\Signature', 'doc_no');
    }

    public function signature_()
    {
    	return $this->belongsTo('App\Signature', 'doc_no');
    }
}
```

Selain array kita bisa pakai

- date
- datetime
- boolean
- integer
- float
- json

Date : 03-07-2024