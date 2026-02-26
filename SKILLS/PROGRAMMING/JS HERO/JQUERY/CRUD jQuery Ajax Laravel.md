#Tech
# Table of Content
- [[#Introduction|Introduction]]
- [[#Read Data|Read Data]]
- [[#Create Data|Create Data]]
- [[#Update Data|Update Data]]
- [[#Delete Data|Delete Data]]

## Introduction
Kita akan membuat master data yg berisi : nama, username, email, password, phone_number dan address. Sebelum kita coding silahkan buat route berikut beserta Controller nya!

routes/web.php

```php
// Index User
Route::get('/dashboard/users', [UserController::class,'index'])->name('users.index');
// Fetch Data User
Route::get('/dashboard/users/data', [UserController::class,'getData'])->name('users.data');
// Create View
Route::get('/dashboard/users/create', [UserController::class,'create'])->name('users.create');
// Logic Create
Route::post('/dashboard/users/store', [UserController::class,'store'])->name('users.store');
// Edit View
Route::get('/dashboard/users/edit/{id}', [UserController::class,'edit'])->name('users.edit');
// Logic Edit
Route::put('/dashboard/users/edit/{id}', [UserController::class,'update'])->name('users.update');
// Delete Logic
Route::delete('/dashboard/users/destroy/{id}', [UserController::class,'destroy'])->name('users.destroy');
```

## Read Data
Di read ini kita akan membuat 2 method di Controller `UserController`,

Sebelumnya jangan lupa untuk download datatable di [sini](https://yajrabox.com/docs/laravel-datatables/10.0)

`App\Http\Controllers\UserController.php`

```php
public function index(){
	return view('pages.dashboard.users.index',[
		'title' => 'Master Data Users'
	]);
}

public function getData()
{
	$users = User::select(['id', 'name', 'username', 'email', 'address', 'phone_number']);
	return DataTables::of($users)->make(true);
}
```

Kita akan ke Views nya. Untuk pemanggilan table kurang lebih sama namun tidak perlu ada value. Jadi cukup name saja kita taruhnya.

```html
<table id="tableUserAjax" class="..." style="width:100%">
	<thead>
		<tr>
			<th>Name</th>
			<th>Username</th>
			<th>Email</th>
			<th>Address</th>
			<th>Phone Number</th>
			<th width="15%">Action</th>
		</tr>
	</thead>
</table>
```

Untuk Logic agak tricky, sebelumnya saya taruh di footer namun tidak bisa. Jadi lebih baik saya taruh di dalam tag head.

```html
@push('ajax')
// Menggunakan sweetalert untuk popup
<script src="<https://cdn.jsdelivr.net/npm/sweetalert2@11>"></script>
// Load datatable jangan lupa load jquery nya
<script type="text/javascript" src="<https://cdn.datatables.net/1.13.7/js/jquery.dataTables.min.js>"></script>
// Script untuk read table
<script type="text/javascript">
$(function(){
	$('#tableUserAjax').DataTable({
		processing: true,
		serverSide: true,
		ajax: '{!! route('users.data') !!}',
		columns: [
			{ data: 'name', name: 'name' },
			{ data: 'username', name: 'username' },
			{ data: 'email', name: 'email' },
			{ data: 'address', name: 'address' },
			{ data: 'phone_number', name: 'phone_number' },
			{
		        name: 'action',
		        orderable: false,
		        searchable: false,
		        render: function (data, type, row) {
                    return `
                    <a class="btn btn-success btn-sm btn-edit" data-id="${row.id}">
				        Edit
				    </a>&nbsp;
					<a class="btn btn-danger btn-sm btn-delete" data-id="${row.id}">
				        Delete
				    </a>
                    `;
                }
			}
		]
	});

});
</script>
// Ketika tombol edit di klik maka akan dilarikan ke halaman /dashboard/users/edit/{id}
<script type="text/javascript">
	$(document).on('click', '#tableUserAjax .btn-edit', function(e) {
    e.preventDefault();
    let id = $(this).data('id');
    // Redirect ke halaman update dengan menggunakan AJAX
    $.ajax({
        url: '/dashboard/users/edit/' + id,
        type: 'GET',
        success: function(response) {
            window.location.href = '/dashboard/users/edit/'+id;
        },
        error: function(xhr, status, error) {
            console.log(xhr.responseText);
        }
    });
});
</script>
// Ketika tombol edit di klik maka akan dilarikan ke halaman /dashboard/users/destroy/{id}
<script type="text/javascript">
	$(document).on('click', '#tableUserAjax .btn-delete', function(e) {
    e.preventDefault();
    let id = $(this).data('id');
    // Hapus data menggunakan AJAX
    if (confirm("Apakah Anda yakin ingin menghapus data ini?")) {
        $.ajax({
            url: '/dashboard/users/destroy/' + id,
            type: 'DELETE',
            success: function(response) {
				Swal.fire({
					icon: "success",
					title: "Berhasil menghapus data!",
					text: "Halaman akan direload dalam 3 detik..."
				});
				window.setTimeout(function(){
						window.location.reload();
				}, 3000);
            },
            error: function(xhr, status, error) {
                console.log(xhr.responseText);
            }
        });
    }
});
</script>
@endpush
```

## Create Data
Logic Create :

`App\Http\Controllers\UserController.php`

```php
public function create(){
	return view('pages.dashboard.users.create',[
		'title' => 'Tambah Users'
	]);
}

public function store(Request $req)
{
	$this->validate($req, [
		'name' => 'required',
		'username' => 'required',
		'email' => 'required|email',
		'phone_number' => 'required',
		'address' => 'required',
	]);

	User::create([
		'name' => $req->name,
		'username' => $req->username,
		'email' => $req->email,
		'phone_number' => $req->phone_number,
		'address' => $req->address,
		'password' => \\\\Hash::make('user123')
	]);

	return response()->json(['success' => true]);
}
```

Kurang lebih create data sama dengan umumnya. Berikut contoh create data views nya..

Formnya

```html
<form id="createUserForm">
	<div class="col-lg-12">
		<label>Nama</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Nama" name="name">
	</div>
	<div class="col-lg-12 mt-3">
		<label>Username</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Username" name="username">
	</div>
	<div class="col-lg-12 mt-3">
		<label>Email</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Email" name="email">
	</div>
	<div class="col-lg-12 mt-3">
		<label>No.Hp</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Nomor HP" name="phone_number">
	</div>
	<div class="col-lg-12 mt-3">
		<label>Alamat</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Alamat" name="address">
	</div>
	<button type="submit" class="btn btn-primary mt-3 mb-3 form-control">Submit</button>
</form>
```

Note : jika terjadi error csrf, matikan saja.

Untuk logic JS nya

```html
@push('ajax')
<script src="<https://code.jquery.com/jquery-3.7.1.min.js>" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
<script src="<https://cdn.jsdelivr.net/jquery.validation/1.16.0/jquery.validate.min.js>"></script>
<script src="<https://cdn.jsdelivr.net/npm/sweetalert2@11>"></script>
<script>
$(document).ready(function(){
	// Menggunakan jQuery Validate
    $('#createUserForm').validate({
        rules: {
            name: 'required',
            username: 'required',
            email: {
                required: true,
                email: true
            },
            phone_number: 'required',
            address: 'required'
        },
        messages: {
            name: 'Nama harus diisi!',
            username: 'Username harus diisi!',
            email: {
                required: 'Email harus diisi!',
                email: 'Harus berformat email!'
            },
            phone_number: 'No.Hp harus diisi!',
            address: 'Alamat harus diisi!',
        },
        submitHandler: function(form){
            $.ajax({
                type: 'POST',
                url: '{{ route('users.store') }}',
                data: $(form).serialize(),
                success: function(response){
                    Swal.fire({
                        icon: "success",
                        title: "Berhasil membuat data!",
                        text: "Halaman akan diredirect dalam 3 detik..."
                    });
                    // 3 detik akan di redirect ke halaman yg dituju
                    window.setTimeout(function(){
                        window.location.href = '{{ route('users.index') }}';
                    }, 3000);
                },
                error: function(error){
                    console.error(error);
                }
            });
            return false;
        }
    });
});
</script>
@endpush
```

## Update Data
Logicnya

```php
public function edit($id)
{
	$user = User::findOrFail($id);
	return view('pages.dashboard.users.edit',[
		'title' => 'Edit Users',
		'user' => $user
	]);
}

public function update(Request $req, $id)
{
	$this->validate($req, [
		'name' => 'required',
		'username' => 'required',
		'email' => 'required|email',
		'phone_number' => 'required',
		'address' => 'required',
	]);

	User::where('id', $id)->update([
		'name' => $req->name,
		'username' => $req->username,
		'email' => $req->email,
		'phone_number' => $req->phone_number,
		'address' => $req->address,
		'password' => \\\\Hash::make('user123')
	]);

	return response()->json(['success' => true]);
}
```

Untuk views nya :

```html
<form id="editUserForm">
	<div class="col-lg-12">
		<label>Nama</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Nama" name="name" value="{{ $user->name }}">
	</div>
	<div class="col-lg-12 mt-3">
		<label>Username</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Username" name="username" value="{{ $user->username }}">
	</div>
	<div class="col-lg-12 mt-3">
		<label>Email</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Email" name="email" value="{{ $user->email }}">
	</div>
	<div class="col-lg-12 mt-3">
		<label>No.Hp</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Nomor HP" name="phone_number" value="{{ $user->phone_number }}">
	</div>
	<div class="col-lg-12 mt-3">
		<label>Alamat</label>
		<input class="form-control form-control-lg" type="text" placeholder="Masukkan Alamat" name="address" value="{{ $user->address }}">
	</div>
	<button type="submit" class="btn btn-success mt-3 mb-3 form-control">Edit</button>
</form>
```

Dan Logic JS nya

```html
@push('ajax')
<script src="<https://code.jquery.com/jquery-3.7.1.min.js>" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
<script src="<https://cdn.jsdelivr.net/jquery.validation/1.16.0/jquery.validate.min.js>"></script>
<script src="<https://cdn.jsdelivr.net/npm/sweetalert2@11>"></script>
<script>
$(document).ready(function(){
    $('#editUserForm').validate({
        rules: {
            name: 'required',
            username: 'required',
            email: {
                required: true,
                email: true
            },
            phone_number: 'required',
            address: 'required'
        },
        messages: {
            name: 'Nama harus diisi!',
            username: 'Username harus diisi!',
            email: {
                required: 'Email harus diisi!',
                email: 'Harus berformat email!'
            },
            phone_number: 'No.Hp harus diisi!',
            address: 'Alamat harus diisi!',
        },
        submitHandler: function(form){
            $.ajax({
                type: 'PUT',
                url: "/dashboard/users/edit/{{ $user->id }}",
                data: $(form).serialize(),
                success: function(response){
                    Swal.fire({
                        icon: "success",
                        title: "Berhasil memperbarui data!"
                    });
                },
                error: function(error){
                    console.error(error);
                }
            });
            return false;
        }
    });
});
</script>
@endpush
```

Note : ketika url get dan post sama (berbeda method), maka secara asinkronus akan langsung berubah.

## Delete Data
Terakhir Delete Data, Logicnya akan seperti ini :

```php
protected function destroy($id){
	$users = User::where('id', $id)->delete();
	return response()->json(['redirect_url' => route('users.index')]);
}
```

Viewsnya sudah dibahas di bagian Read.

Source (Private Repo) : [https://github.com/andarutr/interview-user2-ptpas](https://github.com/andarutr/interview-user2-ptpas)

Date : 17-02-2024