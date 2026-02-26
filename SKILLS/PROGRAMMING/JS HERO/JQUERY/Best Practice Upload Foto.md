![[Upload Foto Jquery Ajax.png]]
#Tech 
# Table of Content
- [[#jQuery Ajax upload foto|jQuery Ajax upload foto]]
- [[#Backendnya|Backendnya]]

## jQuery Ajax upload foto
Kali ini saya mencoba untuk implementasi cara terbaik menurut versi saya. Berikut contoh codingannya...

```javascript
$(document).on("click", "#btnShowModal", function(){
    $("#judulModal").text("Tambah Master Data Bank");

    $("#contentModal").empty().append(`
        <div class="row">
            <div class="col-sm-6">
                <div class="mb-3">
                    <label class="form-label">Nama Bank</label>
                    <input class="form-control" id="namaBankForm">
                </div>
            </div>

            <div class="col-sm-6">
                <div class="mb-3">
                    <label class="form-label">Foto Logo</label>
                    <input type="file" id="pictureForm" class="form-control">
                </div>
            </div>
        </div>

        <button type="button" class="btn btn-primary" id="btnSubmit">Submit</button>
    `);

    $(document).on("click", "#btnSubmit", function(){
        let namaForm = $("#namaBankForm").val();
        let pictureForm = $("#pictureForm")[0].files[0];

        let formData = new FormData();
        formData.append("nama_bank", namaForm);
        formData.append("picture", pictureForm);

        $.ajax({
            type: "POST",
            url: "/su/master/bank/store",
            data: formData,
            processData: false,
            contentType: false,
            success: function(res){
                notifySuccess(res.message);
            },
            error: function(xhr){
                let errors = xhr.responseJSON.errors;
                let message = '';

                $.each(errors, function(key, value) {
                    message += value[0] + '<br>';
                });

                notifyError(message);
            }
        })
    });

    $("#modal").modal("show");
});
```

Penjelasan:
- `namaForm`: Mengambil nilai dari input dengan id `namaBankForm`, yang merupakan nama bank yang dimasukkan oleh pengguna.
- `pictureForm`: Mengambil file yang diunggah oleh pengguna melalui input file dengan id `pictureForm`. `[0].files[0]` digunakan untuk mengambil file pertama yang diunggah oleh pengguna.
- `new FormData()`: Membuat objek FormData yang akan digunakan untuk mengirim data dalam format yang sesuai untuk pengiriman Ajax.
- `formData.append("nama_bank", namaForm)`: Menambahkan nilai `nama_bank` ke objek FormData.
- `formData.append("picture", pictureForm)`: Menambahkan file gambar ke objek FormData.
- `data: formData`: Mengirim data dalam format FormData yang berisi nama bank dan file gambar.
- `processData: false`: Menetapkan `processData` ke `false` agar jQuery tidak memproses data FormData secara otomatis.
- `contentType: false`: Menetapkan `contentType` ke `false` agar jQuery tidak mengatur tipe konten secara otomatis.

## Backendnya
```php
public function store(Request $req)
{
	$validator = Validator::make($req->all(), [
		'nama_bank' => 'required|string|unique:master_bank',
		'picture' => 'required|image|mimes:png'
	],[
		'nama_bank.required' => 'Nama bank harus diisi!',
		'nama_bank.string' => 'Nama bank harus berisi teks!',
		'nama_bank.unique' => 'Nama bank sudah ada!',
		'picture.required' => 'Foto logo bank harus diunggah.',
		'picture.image' => 'File yang diunggah harus berupa gambar.',
		'picture.mimes' => 'Format gambar yang diizinkan hanya: png',
	]);

	if($validator->fails()){
		return response()->json([
			'errors' => $validator->errors()
		], 422);
	}else{
		$picture = $req->file('picture');
		$pictureName = time() . '.' . $picture->getClientOriginalExtension();
		$picture->move(public_path('assets/media/banks'), $pictureName);

		$master_bank = new MasterBank();
		$master_bank->nama_bank = $req->nama_bank;
		$master_bank->picture = $pictureName;
		$master_bank->save();

		return response()->json([
			'message' => 'Berhasil membuat master data bank!'
		]);
	}
}
```

Date: 04-07-2024