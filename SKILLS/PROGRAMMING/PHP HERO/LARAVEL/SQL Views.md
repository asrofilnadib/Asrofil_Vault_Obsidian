#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Studi Kasus|Studi Kasus]]
- [[#Migration|Migration]]
- [[#References|References]]

## Introduction
View merupakan tabel virtual atau tabel logis yang dibangun dari operasi SELECT dan JOIN dari database yang sebenarnya. Dengan kata lain, database view merupakan ilusi (kembar tapi beda) dari tabel sebenarnya.

View dipilih karena memiliki berbagai kelebihan sebagai berikut:
- **View mempermudah query yang kompleks** Bila ingin query data dari 2 atau lebih tabel, kita membutuhkan JOIN. Setiap operasi query yang dibuat ke database mengharuskan kita menulis JOIN lagi dan lagi. Oleh karena itu, View pertama kali dibuat dengan menggunakan JOIN, setelah itu hanya perlu dilakukan operasi yang diinginkan saja kepada View tersebut, tidak perlu ke database langsung.
- **View digunakan untuk mekanisme keamanan** View dibangun dengan memilih beberapa kolom dari beberapa tabel yang diinginkan, tanpa perlu diberikan akses penuh ke database. Hal ini dapat mencegah user untuk mengambil informasi penting dari database sebenarnya.
- **View digunakan untuk menampilkan komputasi data** Bayangkan kita memiliki tabel dengan nama orderDetails dengan tiga kolom yaitu id, kuantitas order, dan harga per order. Perhitungan harga total order tidak baik bila disimpan di dalam database langsung walaupun data tersebut dibutuhkan. Daripada harga total order dihitung terus menerus dalam setiap query, kita bisa menghitung cukup 1x saja dan menyimpan hasilnya dalam database View.

Beberapa kelemahan dari View:
- **Performa** Pada dasarnya, view dibuat dengan menjalankan tambahan query dari database yang ada, apalagi bila view dibentuk dari view yang lainnya.
- **Pembatasan pengubahan data** Semakin banyaknya tabel yang berkolaborasi untuk membentuk view, semakin ketat peraturan untuk mengupdate data dari suatu view.

## Studi Kasus
Sebagai contoh saya memiliki table berikut:

```
1. Users
	- id
	- name
	- email
	- id_role
2. Roles
	- id_role
	- name_role
3. Posts
	- id_post
	- id_user
	- title
	- content
	- updated_at
	- created_at
```

Pada 3 table itu, saya melakukan join inner dalam beberapa halaman. Namun ketika saya join inner table _users_ dengan table _posts_, dalam banyak halaman. Kita akan mengulangi kode kedalam banyak controller. Bagaimana jika 25 halaman? 50 halaman? Bagaimana jika 50 halaman ada beberapa field yang di rename? Misal id_role jadi id_permision? secara kita akan memperbarui join inner 50 halaman tersebut.

> View merupakan tabel virtual atau tabel logis yang dibangun dari operasi SELECT dan JOIN dari database yang sebenarnya. Dengan kata lain, database view merupakan ilusi (kembar tapi beda) dari tabel sebenarnya.

Sebagai contoh pada table posting, kita membutuhkan data :
1. _Nama Publisher_ atau _author_
2. Judul postingannya
3. Isi postingannya
4. Diperbarui
5. Dibuat pada

Kita dapat membuat view sebagai berikut:

```sql
CREATE VIEW view_post_data AS
	SELECT
		posts.id,
		posts.title,
		posts.content,
		posts.updated_at,
		posts.created_at,
		(SELECT name FROM users
			WHERE users.id = posts.id_user
		) AS author,
	FROM posts
```

Intinya kita akan menampilkan nama author tersebut.

## Migration
```php
public function up()
{
	$posts = "CREATE VIEW view_post_data AS
	SELECT
		posts.id,
		posts.title,
		posts.content,
		posts.updated_at,
		posts.created_at,
		(SELECT name FROM users
			WHERE users.id = posts.id_user
		) AS author,
	FROM posts";

	\DB::statement($posts);
}

public function down()
{
	\DB::statement("DROP VIEW IF EXISTS `view_user_data`");
}
```

Untuk penggunaan migration, controller sama dengan penggunaan controller maupun migrate pada umumnya.

## References
- [https://makersinstitute.gitbooks.io/sql/content/sql-view.html](https://makersinstitute.gitbooks.io/sql/content/sql-view.html)