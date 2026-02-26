#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Route|Route]]
- [[#Controller|Controller]]
- [[#Helper|Helper]]

## Introduction
Programmer Junior seperti saya sering kali tidak memperhatikan kualitas kode. Tidak memperhatikan kode dalam artian "YG penting web gue jalan". Saya sadar bahwa dengan menggunakan konsep DRY(Don't Repeat Yourself) dapat mempermudah nanti kita sedang maintenance suatu aplikasi.

Sebelum itu silahkan buat controller resource dan request nya. Kita dalam melakukan crud menggunakan request tanpa `'nama' => $req->nama`.

```
php artisan make:controller BiodataController -r
php artisan make:request BiodataRequest
```

## Route
```php
Route::resource('biodata', BiodataController::class);
```

## Controller
```php
<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Helpers\ServiceHelper as srv;
use App\Http\Requests\BiodataRequest;

class BiodataController extends Controller
{
    public $table = 'biodatas';
    public function index()
    {
        return view('pages.biodata.index',
				['biodatas' => srv::get_data($this->table, 'desc')
			]);
    }

    public function create()
    {
        return view('pages.biodata.create');
    }

    public function store(BiodataRequest $req)
    {
        $data = $req->validated();
        $store = srv::store_data($this->table, $data);
        return redirect()->route('biodata.index');
    }

    public function show($id)
    {
        $biodata = srv::get_detail($this->table, $id);
        return view('pages.biodata.show', compact('biodata'));
    }

    public function edit($id)
    {
        $biodata = srv::get_detail($this->table, $id);
        return view('pages.biodata.edit', compact('biodata'));
    }

    public function update(BiodataRequest $req, $id)
    {
        $data = $req->validated();
        $biodata = srv::update_data($this->table, 'id', $id, $data);
        return redirect()->route('biodata.index');
    }

    public function destroy($id)
    {
        $destroy = srv::destroy_data($this->table, $id);
        return redirect()->back();
    }
}
```

## Helper
Helper ini digunakan untuk keperluan CRUD. Tanpa disadari, fondasi OOP sangat dibutuhkan! Class yg berisikan function untuk menghindari tidak ada perulangan kode.

```php
<?php
namespace App\Helpers;

use Illuminate\Support\Facades\DB;
use Illuminate\Http\Request;

class ServiceHelper
{
    public static function get_data($table, $orderBy)
    {
        $data = DB::table($table)
                    ->orderBy('id', $orderBy)
                    ->get();
        return $data;
    }

    public static function get_detail($table, $param)
    {
        $data = DB::table($table)->find($param);
        return $data;
    }

    public static function get_paginate($table, $orderBy, $paginate)
    {
        $data = DB::table($table)
                    ->orderBy('id', $orderBy)
                    ->paginate($paginate);
        return $data;
    }
	public static function get_paginate_where($table, $orderBy, $col, $con, $paginate)
    {
        $data = DB::table($table)
                    ->orderBy('id', $orderBy)
                    ->where($col, $con)
                    ->paginate($paginate);
        return $data;
    }

    public static function get_limit($table, $orderBy, $limit)
    {
        $data = DB::table($table)
                    ->orderBy('id', $orderBy)
                    ->limit($limit)
                    ->get();
        return $data;
    }

    public static function get_where($table, $col, $con)
    {
        $data = DB::table($table)
                    ->where($col, $con)
                    ->first();
        return $data;
    }

    public static function count_data($table)
    {
        $data = DB::table($table)->count();
        return $data;
    }

	public static function count_where_data($table, $col, $con)
    {
        $data = DB::table($table)
                    ->where($col, $con)
                    ->count();
        return $data;
    }

    public static function store_data($table, $data)
    {
        $store = DB::table($table)->insert($data);
        return $store;
    }

    public static function update_data($table, $col, $con, $data)
    {
        $update_where = DB::table($table)
                            ->where($col, $con)
                            ->update($data);
        return $update_where;
    }

    public static function destroy_data($table, $param)
    {
        $destroy = DB::table($table)
                        ->whereId($param)
                        ->delete();
        return $destroy;
    }

    public static function upload_img(Request $req, $img, $path)
    {
        $file = $req->file($img);
        $file->move($path, $file->getClientOriginalName());
    }
}
```

Penjelasan : serviceHelper
- get_data($table, $orderBy) Menampilkan semua data berdasarkan orderBy/nomor urut
- get_detail($table, $param) Menampilkan berdasarkan parameter
- get_paginate($table, $orderBy, $paginate) Untuk keperluan paginasi
- get_paginate_where($table, $orderBy, $col, $con, $paginate) Untuk keperluan menampilkan data paginasi secara spesifik
- get_limit($table, $orderBy, $limit) Digunakan untuk keperluan limitasi data
- get_where($table, $col, $con) Digunakan untuk menampilkan data spesifik. seperti where('email', Auth::user()->email)
- count_data($table) Digunakan untuk menghitung data di 1 table
- count_where_data($table, $col, $con) Digunakan untuk menghitung data spesifik di 1 table. Misal saya ingin menghitung untuk column yg memiliki is_active = 1
- store_data($table, $data) Digunakan untuk menambah data
- update_data($table, $col, $con, $data) Digunakan untuk memperbarui data spesifik. seperti where('email', Auth::user()->email)
- destroy_data($table, $param) Digunakan untuk menghapus data berdasarkan parameter
- upload_img(Request $req, $img, $path) Digunakan untuk keperluan upload foto.