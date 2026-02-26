#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Migrations|Migrations]]
- [[#Notes|Notes]]

## Introduction
_foreign key_ adalah _pengenal unik_ atau kombinasi pengenal unik yang _menghubungkan dua tabel atau lebih_ dalam suatu database.

Secara umum, _foreign key_ biasa digunakan sebagai penanda hubungan antar tabel. [Tabel](https://kumparan.com/topic/tabel) pertama memiliki peran utama sehingga disebut sebagai primary key di dalamnya, dan tabel kedua merupakan _foreign key_ yang biasa disebut dengan kunci asing.

Suatu tabel dapat dikatakan asing jika terdapat kolom yang merupakan rujukan terhadap tabel utama. Selain itu, ada beberapa fungsi _foreign key_ yang akan dijelaskan di bawah ini.

- _Foreign key_ berfungsi untuk membuat database menjadi konsisten dalam mempertahankan integritas referensi. Maka dari itu, database dapat memonitor setiap data yang akan dimasukkan.
- Ketika kamu telah menetapkan _primary key_ pada tabel utama dan meletakkan _foreign key_ pada tabel kedua, maka akan memudahkanmu untuk melihat rancangan fisik database dengan komponen yang saling terkait. Sehingga, kamu tidak perlu membuat rancangan database secara manual.
- Kolom dalam tabel yang digunakan sebagai _foreign key_ akan memudahkan kamu untuk melakukan pengolahan dari setiap data yang tersimpan dalam database karena [data](https://kumparan.com/topic/data) tersebut sudah saling terkait satu sama lain.
- Membangun hubungan antar baris yang memiliki peran penting dalam normalisasi relasional database. Pada tahap ini, _foreign key_ berfungsi untuk mengakses tabel lain dan menyortir database.

Kesimpulannya, _foreign key_ adalah sebuah atribut atau gabungan atribut yang terdapat dalam suatu tabel yang digunakan untuk menciptakan hubungan atau relasi antara dua tabel.

Referensi : [kumparan.com](https://kumparan.com/how-to-tekno/foreign-key-adalah-pengertian-dan-fungsinya-1xh7ICPpGZa/full)

## Migrations
Sebagai contohnya saya membuat table _likes_ yang terhubung dengan 2 table. table _users_ dan _cards_. Perlu diingat bila table yang ingin kita hubungkan harus berada diatas. Kita ingin menghubungkan table _likes_ dengan table _cards_. **Pastikan posisi file migrations cards diatas table likes**. Secara logika, akan error bila kita ingin menghubungkan table A ke table B yang belum dibuat.

```php
$table->increments('id_like');
$table->unsignedInteger('id_card');
$table->unsignedInteger('id_user');
$table->integer('id_author');
$table->timestamps();

$table->foreign('id_card')->references('id_card')->on('cards');
```

## Notes
Ketika saya ingin menghapus salah satu data di table _cards_, saya mengalami error seperti berikut. Ini artinya parent row tidak bisa dihapus karena ada child row.

```sql
SQLSTATE[23000]: Integrity constraint violation: 1451 Cannot delete or update a parent row: a foreign key constraint fails (`zenku`.`likes`, CONSTRAINT `likes_id_card_foreign` FOREIGN KEY (`id_card`) REFERENCES `cards` (`id_card`)) (SQL: delete from `cards` where `id_card` = 8)
```

Solusinya tambahkan _->onDelete('cascade')_ di child rows
- Parent : _cards_
- Child : _likes_

Sehingga ketika _cards.id_card_ 8, _likes.id_card_ 8 ikut berpengaruh

```php
$table->foreign('id_card')
	->references('id_card')
	->on('cards')
	->onDelete('cascade');
```