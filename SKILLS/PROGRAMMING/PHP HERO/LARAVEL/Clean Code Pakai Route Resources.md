#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Suite to Use|Suite to Use]]
- [[#@method()|@method()]]
- [[#Note|Note]]

## Introduction
Ketika kita sedang routing untuk beberapa keperluan, awalnya tidak memerlukan resource pada route. Di satu sisi kita mungkin akan sangat membutuhkan (Sesuai kebutuhan). Berikut contoh gambar yang tidak pakai resource.

![[L_BestPractice_1.png]]

**Bagaimana bila kita menggunakan resource?** Berikut penerapannya:

```php
Route::resource('materi', MateriController::class);
```

Outputnya akan seperti ini!

note : saya menggunakan package [[Pretty Routes]]

![[L_BestPractice_3.png]]

## Suite to Use
Resource cocok digunakan untuk keperluan CRUD. sebagai contoh guru memiliki akses materi. Guru dapat melihat, membuat, memperbarui maupun menghapus atau biasa disebut _CRUD(Create, Read, Update dan Delete)_.

## @method()
ketika ingin update maupun menghapus data. tambahkan kode berikut dibawah tag form.

```html
<form action="{{ route('...', ['materi' => $id]) }}" method="POST">
@csrf @method('put') atau @method('delete')
//code
</form>
```

## Note
- Bila terdapat keperluan CRUD, gunakan resource.
- Jangan lupa gunakan {{ route() }}