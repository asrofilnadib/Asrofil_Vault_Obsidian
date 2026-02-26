#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Core Build|Core Build]]
- [[#Implementasi|Implementasi]]
	- [[#Implementasi#Penjelasan|Penjelasan]]
- [[#Backendnya (Jika diperlukan)|Backendnya (Jika diperlukan)]]

## Introduction
Bosan menggunakan CKEditor, saya mencoba untuk implementasi WYSYWIG lain. Saya nemu rich editor yg dimana itu open source, [Quill JS](https://quilljs.com/). QuillJS memberikan beberapa ide modern. Yg jelas menurut saya penggunaan QuillJS lebih mudah daripada menggunakan CKEditor. Lebih sederhana.

## Core Build
```html
<link href="<https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.core.css>" rel="stylesheet">
<script src="<https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.core.js>"></script>

<div id="editor">
  <p>Core build with no theme, formatting, non-essential modules</p>
</div>

<script>
  const quill = new Quill('#editor');
</script>
```

## Implementasi
Saya sedang membuat form laporan internship PT.PAS, berikut code lengkapnya.

```html
@extends('master')

@push('styles')
<style>
#clock {
    font-family: Arial, sans-serif;
    font-size: 20px;
    color: #333;
    text-align: right;
}

.form-label{
    font-size: 15px;
}

#kegiatanEditor, #descriptionEditor {
    height: 300px;
    margin-bottom: 20px;
}
</style>
<link href="<https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.snow.css>" rel="stylesheet" />
@endpush

@section('content')
<div class="row">
    <div class="col-lg-10 mx-auto">
        <div class="card">
            <div class="card-header">
                <center>
                    <div id="clock"></div>
                    <img src="<https://i0.wp.com/king-career.com/wp-content/uploads/2024/06/Prakarsa-Alam-Segar.jpg?fit=300%2C300&ssl=1>" class="img-fluid rounded" width="100px">
                    <h1 style="font-size: 25px;">Tambah Data</h1>
                </center>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col-2">
                        <label class="form-label">Date</label>
                    </div>
                    <div class="col-10">
                        <input type="date" class="form-control" id="dateForm">
                    </div>
                    <div class="col-2 mt-3">
                        <label class="form-label">Jam Masuk</label>
                    </div>
                    <div class="col-4 mt-3">
                        <input type="time" class="form-control" id="inForm">
                    </div>
                    <div class="col-2 mt-3">
                        <label class="form-label">Jam Keluar</label>
                    </div>
                    <div class="col-4 mt-3">
                        <input type="time" class="form-control" id="outForm">
                    </div>
                    <div class="col-2 mt-3">
                        <label class="form-label">Kegiatan</label>
                    </div>
                    <div class="col-10 mt-3">
                        <div id="kegiatanEditor"></div>
                    </div>
                    <div class="col-2 mt-3">
                        <label class="form-label">Deskripsi</label>
                    </div>
                    <div class="col-10 mt-3">
                        <div id="descriptionEditor"></div>
                    </div>
                    <div class="col-12 mt-5 text-center">
                        <button class="btn btn-secondary mt-5" style="border-radius: 50px;" onClick="store('draft')">
                            Draft
                        </button>&nbsp;
                        <button class="btn btn-primary mt-5" style="border-radius: 50px;" onClick="store('publish')">
                            Submit
                        </button>
                    </div>
                </div>
            </div>
            <div class="card-footer">
                <a href="/" class="btn btn-success mt-5" style="border-radius: 50px;">
                    Kembali
                </a>
            </div>
        </div>
    </div>
</div>
@endsection

@push('scripts')
<script src="<https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.js>"></script>
<script src="<https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.4/moment.min.js>"></script>
<script>
    const kegiatanEditor = new Quill('#kegiatanEditor', {
        theme: 'snow',
    });

    const descriptionEditor = new Quill('#descriptionEditor', {
        theme: 'snow',
    });

    function notify(title, icon, message){
        Swal.fire({
            title: title,
            icon: icon,
            html: message
        });
    }

    function updateClock() {
        const now = moment().format('HH:mm:ss');
        document.getElementById('clock').textContent = now;
    }

    setInterval(updateClock, 1000);

    updateClock();

    function store(status) {
        let date = $('#dateForm').val();
        let inTime = $('#inForm').val();
        let outTime = $('#outForm').val();
        let activity = kegiatanEditor.root.innerHTML.trim();
        let description = descriptionEditor.root.innerHTML.trim();

        if (!activity || activity === '<p><br></p>') {
            notify("Peringatan", "warning", "Kegiatan tidak boleh kosong.");
            return;
        }

        if (!description || description === '<p><br></p>') {
            notify("Peringatan", "warning", "Deskripsi tidak boleh kosong.");
            return;
        }

        $.ajax({
            type: 'POST',
            url: '/store',
            data: {
                date: date,
                in: inTime,
                out: outTime,
                activity: activity,
                description: description,
                status: status,
            },
            success: function(response) {
                alert('Data berhasil disimpan!');
            },
            error: function(xhr) {
                let errors = xhr.responseJSON.errors;
                let message = '';
                $.each(errors, function(key, value) {
                    message += value[0] + '<br>';
                });

                notify("error", "error", message);
            }
        });
    }
</script>
@endpush
```

### Penjelasan
1. Customize CSS

```css
#kegiatanEditor, #descriptionEditor {
    height: 300px;
    margin-bottom: 20px;
}
```

Ini biar ngga dempet banget.
1. Jangan lupa link css nya

```html
<link href="<https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.snow.css>" rel="stylesheet" />
```

2. Bukan pakai tag textarea

```html
<div id="kegiatanEditor"></div>
```

Kita kebiasa pakai tag textarea, quilljs agak unik.
3. Logicnya

```html
<script src="<https://cdn.jsdelivr.net/npm/quill@2.0.2/dist/quill.js>"></script>
<script>
const kegiatanEditor = new Quill('#kegiatanEditor', {
	theme: 'snow',
});
</script>
```

4. Tangkap value yg kita ketik

```html
<script>
function store() {
	// Kode
	let activity = kegiatanEditor.root.innerHTML.trim();
	// Buatkan validasi tambahan
	if (!activity || activity === '<p><br></p>') {
		notify("Peringatan", "warning", "Kegiatan tidak boleh kosong.");
		return;
	}
}
```

- `kegiatanEditor.root.innerHTML.trim();` Mengambil isi HTML dari editor yang diidentifikasi dengan `kegiatanEditor` dan menghapus spasi di awal dan akhir.
- Mengecek apakah `activity` kosong atau hanya berisi elemen `<p><br></p>`, yang biasanya berarti editor kosong. Jika ya, maka fungsi `notify` akan dipanggil untuk memberi peringatan kepada pengguna dan fungsi `store` akan dihentikan lebih lanjut dengan `return`.

## Backendnya (Jika diperlukan)
```php
public function store(Request $req)
{
	$validator = Validator::make($req->all(), [
		'date' => 'required',
		'in' => 'required',
		'out' => 'required',
	],[
		'date.required' => 'Tanggal harus diisi!',
		'in.required' => 'Jam masuk harus diisi!',
		'out.required' => 'Jam keluar harus diisi!',
	]);

	if($validator->fails()){
		return response()->json([
			'errors' => $validator->errors()
		], 422);
	}else{
		$report = new Report();
		$report->date = $req->date;
		$report->in = $req->in;
		$report->status = $req->status;
		$report->out = $req->out;
		$report->activity = $req->activity;
		$report->description = $req->description;
		$report->save();
	}
}
```

Date: 31-07-2024