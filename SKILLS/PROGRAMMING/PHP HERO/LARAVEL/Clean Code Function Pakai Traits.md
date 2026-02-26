#Tech 
# Table of Content
- [[#Introduction|Introduction]]
	- [[#Introduction#Kelebihan Traits:|Kelebihan Traits:]]
- [[#Traits|Traits]]
- [[#Class Lain|Class Lain]]

## Introduction
Ada case dimana kode tersebut diulang ulang di function lain. Ini membuat kode saya makan space dan ngga reusable. Dengan Traits kita bisa memisahkan kode menjadi 1 bagian.

**Laravel Traits** adalah sebuah fitur dalam Laravel yang memungkinkan kita melakukan **reusable kode** di berbagai class. Traits adalah mekanisme dalam PHP yang memungkinkan Anda untuk menyatukan kembali fungsi-fungsi umum ke dalam beberapa class tanpa menggunakan inheritance (pewarisan) secara langsung.

Dengan menggunakan traits, Anda dapat menghindari masalah "single inheritance" di mana sebuah class hanya bisa mewarisi dari satu parent class. Traits memungkinkan pengembang untuk **mengelompokkan metode-metode umum** dalam satu tempat dan kemudian menggunakannya di beberapa class berbeda.

### Kelebihan Traits:
1. **Kode reusable**: Memudahkan untuk menulis kembali metode-metode yang sering digunakan tanpa duplikasi.
2. **Tidak terbatas inheritance**: Anda bisa menggunakan trait di beberapa class tanpa masalah "single inheritance".
3. **Modularitas**: Kode menjadi lebih modular dan lebih mudah diatur.

## Traits
app/traits/GetCurrentShift.php

```php
<?php

namespace App\Traits;

use App\ShiftDailyNoodle;

trait GetCurrentShift
{
    public function currentShift()
    {
        $currentTime = strtotime(date('H:i:s'));
        $startTime = strtotime("00:00:01");
        $endTime = strtotime("06:59:00");

        $master_shift = ShiftDailyNoodle::orderBy('id', 'DESC')->first();

        if ($master_shift->schedule == 'NS') {
            if ($currentTime >= strtotime("06:59:01") && $currentTime <= strtotime("15:00:00")) {
                $shift = 1;
                $tgl_sekarang = date('Y-m-d');
            } elseif ($currentTime >= strtotime("15:00:01") && $currentTime <= strtotime("23:00:00")) {
                $shift = 2;
                $tgl_sekarang = date('Y-m-d');
            } elseif ($currentTime >= strtotime("23:00:01") && $currentTime <= strtotime("23:59:00")) {
                $shift = 3;
                $tgl_sekarang = date('Y-m-d');
            } elseif ($currentTime >= $startTime && $currentTime <= $endTime) {
                $tgl_sekarang = date('Y-m-d', strtotime('-1 day'));
                $shift = 3;
            }
        } else {
            if ($currentTime >= strtotime("06:30:01") && $currentTime <= strtotime("11:59:00")) {
                $shift = 1;
                $tgl_sekarang = date('Y-m-d');
            } elseif ($currentTime >= strtotime("12:00:01") && $currentTime <= strtotime("17:00:00")) {
                $shift = 2;
                $tgl_sekarang = date('Y-m-d');
            } elseif ($currentTime >= strtotime("17:00:01") && $currentTime <= strtotime("22:01:00")) {
                $shift = 3;
                $tgl_sekarang = date('Y-m-d');
            }
        }

        $prevDate = $shift == 1 ? date('Y-m-d', strtotime('-1 day')) : $tgl_sekarang;

        return compact('shift', 'tgl_sekarang', 'prevDate');
    }
}

```

Intinya kita compact variable mana saja yg ingin kita gunakan di class lain.

## Class Lain

```php
use App\Traits\GetCurrentShift;

class DashboardStockController extends Controller
{
    use GetCurrentShift;

	public function getData(Request $req)
	{
		// nama function
	    $shiftData = $this->currentShift();
	    $shift = $shiftData['shift'];
	    $tgl_sekarang = $shiftData['tgl_sekarang'];
	    $prevDate = $shiftData['prevDate'];
	    // ...
   }
}
```

Date: 13-09-2024