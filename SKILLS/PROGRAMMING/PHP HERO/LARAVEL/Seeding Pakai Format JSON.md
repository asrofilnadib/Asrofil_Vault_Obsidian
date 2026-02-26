#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
umumnya saya seeding banyak data pada file Seeder yg sama . seperti berikut :

```php
$users=  array(
[
	"nama" => "Andaru",
	"umur" => 22,
	...............
]
);

\DB::user()->insert($users);
```

Bagaimana jika kita ingin seeding data 10?100?1000? Jelas tidak akan efektif. Seeding menggunakan json _solusinya_!. Berikut caranya:
- Buat folder baru di folder database dengan nama data (bebas),
- buat file json dengan data seperti berikut (disesuaikan kebutuhan)

users.json

```json
[
	{"nama": "Andaru",'umur': 22}
]
```

- Pada seeder, kodingan akan seperti berikut:

```php
$json = \File::get('database/data/users.json');
$users = json_decode($json);

foreach($users as $user){
	\DB::table('users')->insert([
		'nama' => $user->nama,
		'umur' => $user->umur
	]);
}
```