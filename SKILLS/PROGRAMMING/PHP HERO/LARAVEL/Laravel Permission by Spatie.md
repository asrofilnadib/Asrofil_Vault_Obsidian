#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Installation|Installation]]
	- [[#Installation#Service Providers|Service Providers]]
	- [[#Installation#Publish|Publish]]
	- [[#Installation#Clear Cache|Clear Cache]]
- [[#Traits Models|Traits Models]]
- [[#Struktur Table|Struktur Table]]
	- [[#Struktur Table#Roles|Roles]]
	- [[#Struktur Table#Model Has Roles|Model Has Roles]]
	- [[#Struktur Table#Permissions|Permissions]]
	- [[#Struktur Table#Role Has Permissions|Role Has Permissions]]
	- [[#Struktur Table#Model Has Permission|Model Has Permission]]
- [[#Roles|Roles]]
	- [[#Roles#Membuat Roles|Membuat Roles]]
	- [[#Roles#Menghubungkan Roles Dan Permissions|Menghubungkan Roles Dan Permissions]]
	- [[#Roles#Menghubungkan Role Dan Model|Menghubungkan Role Dan Model]]
	- [[#Roles#Menghapus Role|Menghapus Role]]
- [[#Permissions|Permissions]]
	- [[#Permissions#Membuat Permissions|Membuat Permissions]]
	- [[#Permissions#Menghubungkan Permissions Dan Roles|Menghubungkan Permissions Dan Roles]]
	- [[#Permissions#Menghubungkan Role Dan Permissions|Menghubungkan Role Dan Permissions]]
	- [[#Permissions#Menghapus Permissions|Menghapus Permissions]]
- [[#Blade Directives|Blade Directives]]
	- [[#Blade Directives#Permissions|Permissions]]
- [[#Artisan|Artisan]]
	- [[#Artisan#Membuat Role dan Permissions|Membuat Role dan Permissions]]
	- [[#Artisan#Displaying Role dan Permissions|Displaying Role dan Permissions]]
- [[#The Controller|The Controller]]
- [[#Studi Kasus|Studi Kasus]]
	- [[#Studi Kasus#Login|Login]]

## Introduction
Laravel Permission Role digunakan untuk membuat perizinan pada role. Saya biasa menggunakan package milik [spatie.be](http://spatie.be/)

## Installation
Buka terminal, ketik :

```
composer require spatie/laravel-permission
```

### Service Providers
```php
'providers' => [
    // ...
    Spatie\Permission\PermissionServiceProvider::class,
];
```

### Publish
```shell
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

### Clear Cache
```
php artisan optimize:clear
 # or
php artisan config:clear
```

## Traits Models
Sebelum menggunakannya, kita harus traits model. Sebagai contoh saya menggunakan model User

```php
use Illuminate\Foundation\Auth\User as Authenticatable;
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasRoles;
    // ...
}
```

## Struktur Table
### Roles
Table ini digunakan untuk membuat roles beserta guard nya. Sebagai contoh : id 1 berisikan role Admin.

```
tb : roles
- id (int)
- name (varchar 255)
- guard_name (varchar 255)
- created_at (timestamp)
- updated_at (timestamp)
```

### Model Has Roles
Berisikan user beserta role yang dimilikinya. Sebagai contoh : user dengan id 1 itu admin. Berarti dia memiliki role_id 1.

```
tb: model_has_roles
- role_id (int)
- model_type (varchar 255)
- model_id (int)
```

### Permissions
Table ini digunakan untuk membuat permission atau perizinan beserta guard nya. Sebagai contoh : membuat permission untuk melakukan hapus data pengguna.

```
tb: permissions
- id (int)
- name (varchar 255)
- guard_name (varchar 255)
- created_at (timestamp)
- updated_at (timestamp)
```

### Role Has Permissions
Seperti nama nya, table ini berisikan role beserta permission nya. Sebagai contoh : hanya role admin yang memiliki akses untuk menghapus data pengguna.

```
tb: role_has_permissions
- permission_id (int)
- role_id (int)
```

### Model Has Permission
Berisikan model beserta permission nya. Sebagai contoh : beberapa user memiliki hak akses / permission untuk menghapus data pengguna.

```
tb: model_has_permissions
- permission_id (int)
- model_type (varchar 255)
- model_id (int)
```

## Roles
### Membuat Roles
```php
use Spatie\Permission\Models\Role;

$role = Role::create(['name' => 'admin']);
```

### Menghubungkan Roles Dan Permissions
otomatis akan membuat data di table `role_has_permissions`

```php
$role->givePermissionTo($permission);
```

### Menghubungkan Role Dan Model
Secara otomatis data akan masuk di table `model_has_roles`

```php
$user = User::create([
	....................
]);
$user->assignRole('admin');
```

### Menghapus Role
```php
$permission->removeRole($role);
```

## Permissions
### Membuat Permissions

```php
use Spatie\Permission\Models\Permission;

$permission = Permission::create(['name' => 'delete articles']);
```

### Menghubungkan Permissions Dan Roles
```php
$permission->assignRole($role);
```

### Menghubungkan Role Dan Permissions
Secara otomatis akan dibuatkan data di tabel `model_has_permissions`

```php
$user = User::create([
	....................
]);

$user->givePermissionTo('delete articles');
```

### Menghapus Permissions

```php
$role->revokePermissionTo($permission);
```

The `HasRoles` trait adds Eloquent relationships to your models, which can be accessed directly or used as a base query:

```php
// get a list of all permissions directly assigned to the user
$permissionNames = $user->getPermissionNames(); // collection of name strings
$permissions = $user->permissions; // collection of permission objects

// get all permissions for the user, either directly, or from roles, or from both
$permissions = $user->getDirectPermissions();
$permissions = $user->getPermissionsViaRoles();
$permissions = $user->getAllPermissions();

// get the names of the user's roles
$roles = $user->getRoleNames(); // Returns a collection
```

## Blade Directives
### Permissions
Kita dapat menggunakan @can untuk memperbolehkan suatu user mengakses fitur kita.

```
@if(auth()->user()->can('edit articles') && $some_other_condition)
  //
@endif
```

## Artisan
### Membuat Role dan Permissions
```
php artisan permission:create-role writer
```

```
php artisan permission:create-permission "edit articles"
```

Membuat role, sekaligus menghubungkan ke permissions nya

```
php artisan permission:create-role writer web "create articles|edit articles"
```

### Displaying Role dan Permissions
```
php artisan permission:show
```

## The Controller
```php
public function __construct()
{
  $this->middleware(['role:admin','permission:create articles']);
}
```

## Studi Kasus
### Login
```php
if(auth()->attempt(['username' => $request->username,'password' => $request->password]))
{
  $user = Auth::user();
  if($user->hasRole('admin')){
		.............
  }elseif($user->hasRole('guru')){
		.............
  }
```

> Jika role access manual menggunakan id_role, ketika kita menggunakan laravel-permission tidak menggunakan id_role pada table users