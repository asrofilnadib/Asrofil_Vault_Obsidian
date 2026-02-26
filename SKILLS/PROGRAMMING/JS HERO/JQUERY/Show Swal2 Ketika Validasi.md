![[Show Validate Message.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#HTML nya|HTML nya]]
- [[#Logic JS nya|Logic JS nya]]
- [[#Logic PHP|Logic PHP]]

## Introduction
Sebelumnya saya jika error pesan nya itu manual di ajax, namun ternyata kita bisa ambil response json dari Backend dan ditampilkan ke frontend.

## HTML nya
```html
<div class="modal fade" id="modalData" aria-hidden="true" aria-labelledby="..." tabindex="-1">
    <div class="modal-dialog modal-dialog-scrollable">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="modalDataLabel">Tambah Data</h5>
		<button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body text-center">
                <div class="mt-1">
                    <form id="storeForm">
                        <div class="row">
                            <div class="col-lg-6">
                                <label for="">Kode Admin</label>
                                <select id="kode_admin" class="">
                                    <option value="">Pilih Kode Admin</option>
                                </select>
                            </div>
                            <div class="col-lg-6">
                                <label for="">Nama Admin</label>
                                <select id="nama_admin" class="">
                                    <option value="">Pilih Nama Admin</option>
                                </select>
                            </div>
                            <div class="col-lg-12">
                                <input type="hidden" id="editId">
                                <button class="btn btn-primary rounded-pill">
                                    Submit
                                </button>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
```

## Logic JS nya
```javascript
$("#storeForm").submit(function(e){
	e.preventDefault();
	let id = $("#editId").val();
	let url = id ? "/hr-connect/masters/admin/" + id : "/hr-connect/masters/admin/store";
	let formData = {
		"kode_admin": $("#kode_admin").val(),
		"nama_admin": $("#nama_admin").val(),
	};

	$.ajax({
		type: "POST",
		url: url,
		data: formData,
		success: function(res){
			table.api().draw();

			if(res.status == 'success'){
				Swal.fire({
					icon: 'success',
					title: 'Sukses',
					text: res.message
				})
			}

			$("#modalData").modal("hide");
		},
		error: function(xhr){
			let errors = xhr.responseJSON.errors;
			let message = '';
			$.each(errors, function(key, value) {
				message += value[0] + '<br>';
			});

			Swal.fire({
				icon: 'error',
				title: 'Error',
				html: message,
			});
		}
	});
});
```

## Logic PHP
```php
public function store(Request $req)
{
	$validator = Validator::make($req->all(), [
		'kode_admin' => 'required',
		'nama_admin' => 'required'
	], [
		'kode_admin.required' => 'Kode admin harus diisi!',
		'nama_admin.required' => 'Nama admin harus diisi!',
	]);

	if($validator->fails()){
		return response()->json([
			'status' => 'error',
			'errors' => $validator->errors(),
		], 422);
	}else{
		$nik_admin = User::where('name', $req->nama_admin)->first();

		AdminDepartment::create([
			"kode_admin" => $req->kode_admin,
			"nik_admin" => $nik_admin->username,
			"nama_admin" => $req->nama_admin,
		]);

		return response()->json([
			'status' => 'success',
			'message' => 'Berhasil tambah data masters admin!'
		]);
	}
}
```