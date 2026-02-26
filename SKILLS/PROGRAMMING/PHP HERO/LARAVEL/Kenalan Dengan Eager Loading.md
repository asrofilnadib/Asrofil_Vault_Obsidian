#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Contoh Eager Loading Di Laravel 7|Contoh Eager Loading Di Laravel 7]]
- [[#Penggunaan Eager Loading|Penggunaan Eager Loading]]
- [[#Kesimpulan|Kesimpulan]]

## Introduction
**Eager Loading** di Laravel adalah teknik untuk mengurangi jumlah query yang dijalankan oleh ORM Eloquent dengan memuat relasi secara bersamaan. Ini sangat berguna untuk meningkatkan performa ketika berurusan dengan banyak data yang berelasi.

## Contoh Eager Loading Di Laravel 7
Misalkan kamu memiliki dua model: **Post** dan **Comment**, di mana satu **Post** memiliki banyak **Comment** (one-to-many relationship). Berikut contoh cara menggunakan eager loading.

```php
// Post.php
class Post extends Model
{
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
}

// Comment.php
class Comment extends Model
{
    public function post()
    {
        return $this->belongsTo(Post::class);
    }
}
```

## Penggunaan Eager Loading
```php
$posts = Post::with('comments')->get();

$posts = Post::with(['comments' => function ($query) {
    $query->where('is_active', 1);
}])->get();

$posts = Post::with(['comments', 'author'])->get();
```

```html
@foreach ($posts as $post)
    <h2>{{ $post->title }}</h2>

    <h4>Comments:</h4>
    <ul>
        @foreach ($post->comments as $comment)
            <li>{{ $comment->content }}</li>
        @endforeach
    </ul>
@endforeach
```

## Kesimpulan
Eager loading sangat berguna dalam aplikasi Laravel untuk meminimalkan jumlah query yang dieksekusi dan mengurangi overhead yang terjadi akibat N+1 query problem. Dengan menggunakan `with()` dan `load()`, kamu bisa memuat relasi secara efisien sesuai dengan kebutuhan aplikasimu.

Date: 08-10-2024