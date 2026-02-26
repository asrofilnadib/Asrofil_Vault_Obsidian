#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Installation|Installation]]
- [[#Fit Image|Fit Image]]

## Introduction
Gambar adalah salah satu pemanis pada website. Pentingnya penggunaan website membuat developer lebih memperhatikan keperluan penggunaan gambar. Disatu sisi gambar yang memiliki size yang besar dapat berpengaruh kepada performa website kita. Dengan adanya [intervention image](https://image.intervention.io/v2/introduction/installation), packages yang berfungsi untuk me-manipulasi gambar dapat menjaga performa website kita agar lebih stabil.

## Installation
```
composer require intervention/image

```

**config/app.php**

```php
'providers' => [
	....
	Intervention\Image\ImageServiceProvider::class,
],

'aliases' => [
	....
	'Image' => Intervention\Image\Facades\Image::class,
]

```

Lalu, ketik perintah artisan dibawah :

```
php artisan vendor:publish --provider="Intervention\\\\Image\\\\ImageServiceProviderLaravelRecent"

```

## Fit Image
Sebagai contoh bila pengguna ingin melakukan upload foto profile nya sebesar 1920 x1080 (estimasi 500anKB). Dari segi ukuran mungkin sudah kecil, namun dalam segi pixels sangat boros. sedangkan kita hanya menampilkan foto tersebut pada navbar. Jika di Fit akan seperti ini:
- Foto Asli (1920x1080) = 480Kb
- Foto Manipulasi (250x250) = 18Kb

Pada controller seperti ini :

```php
use Intervention\Image\Facades\Image;

if($req->hasFile('picture'))
{
	$file = $req->file('picture');
	$imanip = Image::make($file)->fit(250);
	$imanip->save('img/profil/'.$file->getClientOriginalName());
	//Code
}

```

Terdapat banyak sekali bentuk manipulasi gambar selain _Fit()_, seperti : width,resize, stream, dan masih banyak lainnya. Selengkapnya silahkan kunjungi dokumentasinya [disini](https://image.intervention.io/v2/introduction/installation)