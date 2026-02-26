#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#JSON di MySQL|JSON di MySQL]]
- [[#Migrations nya|Migrations nya]]
- [[#HTML nya|HTML nya]]
- [[#Logicnya|Logicnya]]
- [[#Backendnya|Backendnya]]

## Introduction
Melanjutkan project sebelumnya, E-Resepsionis. Kali ini ada yg unik. Yaitu menggunakan data type JSON di database. Kenapa pakai json? karena pada penandatangan ini saya tidak tahu pastinya berapa batas maksimal penandatangan. maka dari itu saya menggunakan JSON.

## JSON di MySQL
Tipe data JSON digunakan untuk menyimpan data dalam format JSON (JavaScript Object Notation). Tipe data ini _memungkinkan_ _penyimpanan data terstruktur_ dengan format yang _fleksibel_ dan _efisien_ dalam kolom tabel.

Alasan saya menggunakan type data ini adalah: **Penyimpanan Terstruktur**:
- JSON memungkinkan penyimpanan _data terstruktur dalam satu kolom_, mirip dengan dokumen dalam database NoSQL. Ini sangat berguna untuk aplikasi yang membutuhkan penyimpanan data _dinamis dan tidak terstruktur_.

## Migrations nya
```php
$table->json('progress');
```

## HTML nya
```html
<div id="penandatangan-container">
    <div class="form-group row penandatangan">
        <label class="col-sm-2 col-form-label text-right">Penandatangan 1</label>
        <div class="col-sm-8">
            <select class="form-control select2" id="progress[]">
                <option value="">Pilih</option>
                @foreach($users as $user)
                    <option value="{{ $user->id }}">{{ $user->name }}</option>
                @endforeach
            </select>
        </div>
        <div class="col-sm-2">
            <button type="button" class="btn btn-danger btn-sm remove-penandatangan form-control">Hapus</button>
        </div>
    </div>
</div>
<div class="form-group row">
    <div class="col-sm-2"></div>
    <div class="col-sm-10">
        <button type="button" class="btn-sm btn btn-info mb-3" id="add-penandatangan" style="border-radius: 10px;">
            + Tambah Penandatangan
        </button>
    </div>
</div>
```

Penjelasan:
- Kita akan melakukan append di class `penandatangan`.
- Menggunakan id berbentuk array `progress[]`

## Logicnya
```javascript
$('#add-penandatangan').on('click', function() {
    let count = $('#penandatangan-container .penandatangan').length + 1;
    let newPenandatangan = `
        <div class="form-group row penandatangan">
            <label class="col-sm-2 col-form-label text-right">Penandatangan ${count}</label>
            <div class="col-sm-8">
                <select class="form-control select2" name="penandatangan[]">
                    <option value="">Pilih</option>
                    @foreach($users as $user)
                        <option value="{{ $user->id }}">{{ $user->name }}</option>
                    @endforeach
                </select>
            </div>
            <div class="col-sm-2">
                <button type="button" class="btn btn-danger btn-sm remove-penandatangan form-control">Hapus</button>
            </div>
        </div>
    `;
    $('#penandatangan-container').append(newPenandatangan);
});

$(document).on('click', '.remove-penandatangan', function() {
    $(this).closest('.penandatangan').remove();
});
```

Penjelasan:
- Ketika di klik tambah penandatangan, maka kolom akan bertambah,'
- Di logic js nya ini saya menggunakan name di tag selectnya `penandatangan[]`

Perhatikan Logic store datanya. Agak tricky!

```javascript
function store(){
    let keterangan = $("#keterangan").val();
    let progress = [];

    $('#penandatangan-container .penandatangan select').each(function() {
        progress.push($(this).val());
    });

    $.ajax({
        type: "POST",
        url: "/e-resepsionis/admin/pengiriman-berkas/store",
        data: {
            progress: progress,
            description: keterangan
        },
        success: function(res){
            Swal.fire({
                icon: "success",
                title: "Berhasil",
                text: res.message
            });

            setTimeout(() => {
                window.location.href = "/e-resepsionis/admin";
            }, 1000);
        },
        error: function(xhr){
            // lihat dokumen Menampilkan Validasi Laravel ke Ajax
        }
    })
}
```

Penjelasan:
- kita akan membuat array kosong nanti di push ke array tsb `let progress = [];`
- value nya kita foreach seperti ini

```javascript
$('#penandatangan-container .penandatangan select').each(function() {
	progress.push($(this).val());
});
```

- Otomatis value nya akan di push ke variable progress.
- Terakhir passing ke ajax seperti biasa.

## Backendnya
```php
$progressIds = $req->input('progress', []);
$description = $req->input('description', '');

$progressDetails = DB::table('users')
    ->whereIn('id', $progressIds)
    ->get(['id', 'name', 'username'])
    ->map(function($user) use ($progressIds) {
        $index = array_search($user->id, $progressIds) + 1;
        return [
            'urutan' => $index,
            'name' => $user->name,
            'username' => $user->username,
            'status' => 'belum'
        ];
    });

DB::table('document_progress')
    ->insert([
        'tanggal' => Carbon::parse(now())->format('Y-m-d'),
        'user_id' => auth()->id(),
        'progress' => json_encode($progressDetails),
        'description' => $description,
        'status' => 'dibuat',
        'created_at' => now(),
        'updated_at' => now(),
    ]);

return response()->json([
    'message' => 'Berhasil membuat berkas!'
]);
```

Penjelasan:
- kita akan menangkap value progress dengan `$progressIds = $req->input('progress', []);`

```php
$progressDetails = DB::table('users')
->whereIn('id', $progressIds)
->get(['id', 'name', 'username'])
->map(function($user) use ($progressIds) {
	$index = array_search($user->id, $progressIds) + 1;
	return [
		'urutan' => $index,
		'name' => $user->name,
		'username' => $user->username,
		'status' => 'belum'
	];
});
```

- whereIn ini kita akan mencari id tapi dalam bentuk json. WhereIn sendiri itu biasanya digunakan untuk membatasi hasil query berdasarkan _nilai kolom yang cocok dengan salah satu nilai dalam array_ yang diberikan.
- `$index = array_search($user->id, $progressIds) + 1;` kita akan mencari array jika benar maka akan ditambah 1.
- saya akan melakukan return urutan, name, username, status ke dalam kolom progress pada table `document_progress`.
- Terakhir jangan lupa json nya di encode! `json_encode($progressDetails)`

Date: 03-08-2024