#Tech 
## Introduction
Ada case dimana gue interview orang dengan pertanyaan "Mas unsigned big integer untuk apa ya?". Lesgoooo

## Untuk Apa?
Di Laravel, tipe kolom **`unsignedBigInteger`** digunakan untuk membuat kolom di database yang:
- Berisi **bilangan bulat (integer) positif** (karena _unsigned_ berarti tidak boleh negatif),
- Umumnya dipakai untuk **foreign key** yang mengacu ke kolom `id` dari tabel lain, karena kolom `id` di Laravel secara default bertipe `bigIncrements` (yang setara dengan `unsignedBigInteger`).

## Implementasi
```php
$table->unsignedBigInteger('user_id');
$table->foreign('user_id')->references('id')->on('users');
```

Date: 24-10-2025