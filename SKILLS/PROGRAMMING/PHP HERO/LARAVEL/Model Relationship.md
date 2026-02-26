#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Migration|Migration]]
- [[#Model|Model]]
- [[#Controller|Controller]]
- [[#Views|Views]]

## Introduction
Eloquent Relationship merupakan method yg didefinisikan di dalam model dan digunakan untuk menghubungkan antar tabel yg saling berhubungan. Ini adalah cara efektif untuk melakukan relasi database tanpa join inner `join()`. Kali ini saya akan membuat case sederhana, terdapat biodata yg memiliki relasi _one to one_ dan _one to many_

![[Eloquent Relationship.png]]

## Migration
Pertama tama buat migration berikut
1. biodata
2. tags
3. phone

**migration biodatas**

```php
public function up(): void
{
	Schema::create('biodatas', function (Blueprint $table) {
		$table->id();
		$table->string('nama', 25);
		$table->tinyInteger('umur');
		$table->string('hobi', 25);
		$table->timestamps();
	});
}
```

**migration tags**

```php
public function up(): void
{
	Schema::create('tags', function (Blueprint $table) {
		$table->id();
		$table->unsignedBigInteger('biodata_id');
		$table->string('name_tag', 25);
		$table->timestamps();
		$table->foreign('biodata_id')->references('id')->on('biodatas')->onDelete('cascade');
	});
}
```

Note : perhatikan `unsignedBigInteger()`

**migration phones**

```php
public function up(): void
{
	Schema::create('phones', function (Blueprint $table) {
		$table->id();
		$table->unsignedBigInteger('biodata_id');
		$table->string('phone_number', 25);
		$table->timestamps();

		// Foreign Key
		$table->foreign('biodata_id')->references('id')->on('biodatas')->onDelete('cascade');
	});
}
```

## Model
**Model Biodata**

```php
class Biodata extends Model
{
    use HasFactory;
    protected $guarded = [];

    public function phone()
    {
        return $this->hasOne(Phone::class);
    }

    public function tag()
    {
        return $this->hasMany(Tag::class);
    }
}
```

Table biodatas melakukan relasi _one to one_ ke table phones dan _one to many_ ke table tags.

**Model Tag**

```php
class Tag extends Model
{
    use HasFactory;

    public function biodata()
    {
        return $this->belongsTo(Biodata::class);
    }
}
```

**Model Phone**

```php
class Phone extends Model
{
    use HasFactory;

    public function biodata()
    {
        return $this->belongsTo(Biodata::class);
    }
}
```

Selanjutnya buat beberapa data di table biodatas, phones, dan tags.

## Controller
**Controller Biodata**

```php
public function index()
{
	return view('pages.biodata.index',[
		'biodatas' => Biodata::latest()->get()
	]);
}

public function show($id)
{
	$biodata = Biodata::where('id',$id)->first();
	return view('pages.biodata.show', compact('biodata'));
}
```

Note: tidak semua function bisa menggunakan relationship. `latest()->get()` dapat melakukannya!

## Views
**index.html**

```html
@foreach($biodatas as $bio)
<tr>
  <td>{{ $bio->nama }}</td>
  <td>{{ $bio->phone->phone_number }}</td>
  <td>{{ $bio->umur }}</td>
  <td>{{ $bio->hobi }}</td>
  <td>
	<a href="{{ route('biodata.show', $bio->id) }}" class="btn btn-sm btn-primary">Show</a>
  </td>
</tr>
@endforeach
```

Untuk memanggil nya kita cukup looping `$var->nama_method->field`

**show.html**

```html
<p>{{ $biodata->nama }}</p>
<p>{{ $biodata->umur }}</p>
<p>{{ $biodata->hobi }}</p>
@foreach ($biodata->tag()->get() as $item)
<ul>
    <li>{{ $item->name_tag }}</li>
</ul>
@endforeach
```

Untuk memanggil _one to many_, kita harus looping terlebih dahulu. `$var->nama_method()->get()` . Kesimpulan nya adalah dengan menggunakan Eloquent Relationship, kita dapat koding secara efisien tanpa melakukan join inner. Jika menggunakan join inner, lalu ada field yg harus kita ubah. Akan cukup sulit dan yg jelas proses maintenance akan lebih buruk.