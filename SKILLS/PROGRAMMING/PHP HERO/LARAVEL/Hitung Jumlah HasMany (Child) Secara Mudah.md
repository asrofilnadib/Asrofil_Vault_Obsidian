![[Pasted image 20250719153112.png]]
#Tech 
## Introduction
Ada case dimana gue harus hitung sub menu dari suatu menu. ternyata ada cara mudahnya. Pakai `withCount('nama_relasi')`.
## Implementasi
```php
public function index()
{
	$data['menus'] = AuthMenu::withCount('sub_menu')->get();
	return view('master.menu', $data);
}
```

```html
// HTML NYA
@foreach($menus as $menu)
<tr>
	<td>{{ $menu->title }} <span class="badge bg-danger text-white" onClick="removeMenu({{ $menu->id }})">hapus</span></td>
	<td>{{ $menu->sub_menu_count }}</td>
	<td>{{ $menu->permission }}</td>
	<td>{{ $menu->url }}</td>
</tr>
@endforeach
```

Perlu di note ada yg menarik. ternyata di blade cukup tambahkan `_count` setelah nama relasi otomatis si Laravel ini akan count. Nah contoh relasi nya.

```php
class AuthMenu extends Model
{
	protected $table = 'auth_menu';
	protected $guarded = [];

	public function sub_menu()
	{
		return $this->hasMany(AuthSubMenu::class, 'auth_menu_id','id');
	}
}
```

Sederhana tapi berguna...

Date: 19-07-2025