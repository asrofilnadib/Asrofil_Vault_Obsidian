<img src="IMG_20240729_171050.jpg" width="450" align="left">














#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Implementasi|Implementasi]]
	- [[#Implementasi#Input dan logicnya|Input dan logicnya]]
	- [[#Implementasi#Scan|Scan]]
	- [[#Implementasi#Tampilkan table|Tampilkan table]]

## Introduction
Kali ini saya disuruh buat aplikasi khusus untuk keperluan kirim berkas dan pantau status berkas (sudah selesai atau belum) untuk admin (semua departemen) dan resepsionis. Nama aplikasinya E-Resepsionis. Kurang lebih looksnya seperti ini:

![[E-Resepsionis(Resepsionis)_2.png]]

![[E-Resepsionis(Resepsionis)_3.png]]

Sebelum itu RFID adalah Radio Frequency Identification, adalah teknologi yang menggunakan _gelombang radio_ untuk _mentransfer data antara sebuah perangkat_ dan tag atau label yang dipasang pada objek. Tujuan utamanya adalah untuk mengidentifikasi dan melacak objek secara otomatis.

RFID terdiri dari dua komponen utama:
1. **Tag RFID**: Ini adalah perangkat kecil yang terpasang pada objek. Tag RFID memiliki chip untuk menyimpan informasi dan antena untuk menerima dan mengirim sinyal radio. Tag dapat berupa tag aktif (memiliki sumber daya sendiri seperti baterai) atau tag pasif (mengandalkan energi dari sinyal pembaca).
2. **Pembaca RFID**: Ini adalah perangkat yang mengirimkan sinyal radio untuk berkomunikasi dengan tag. Pembaca menerima data dari tag dan dapat mengirimkannya ke sistem komputer atau database untuk diproses.

## Implementasi
### Input dan logicnya
```html
<div class="col-lg-6 mx-auto">
	<center>
		<input type="text" id="scanBerkasMasuk" class="form-control" placeholder="Scan"/>
	</center>
</div>

<script>
$('#scanBerkasMasuk').keypress(function(e)
{
	if(e.which == 13) {
		let rfid = $('#scanBerkasMasuk').val();
		if(rfid == '') {
			toastr.warning('Silahkan Scan ID Card');
			return false;
		}

		$.ajax({
			type: "GET",
			url: "/e-resepsionis/berkas-masuk/scan",
			data: {
				rfid
			},
			success: function(res){
				if(res.success){
					sessionStorage.setItem("eResepsionisNik", res.nik);
					getData(defaultFrom, defaultTo);

					$("#filterVisible").show();
					$("#tableBerkasMasuk").show();
					// toastr.warning(res.nik);
				}else{
					toastr.warning('RFID Card tidak ditemukan!');
				}
			}
		});
	}
});
</script>
```

Intinya kita akan menangkap value. Jadi flashdisk itu penangkap rfid dimana ketika kita scan card pegawai maka Otomatis input terisi dengan angka rfid kamu. Angka ini diolah ke backend dan lain-lain.

### Scan
```php
public function scanBerkas(Request $req)
{
	$rfid = (int)$req->rfid;
	$user = DB::connection('192.168.154.xx')
			->table('MSIDCARD')
			->select('NIK')
			->where(['CARDNODEVICE' => $rfid])
			->value('NIK');

	if($user){
		if(substr($user,0,3) == '105' || substr($user,0,3) == '205') {
			$nik = $user;
		}else{
			$nik  = explode('-', $user)[1];
		}

		return response()->json([
			'success' => true,
			'nik' => $nik
		]);
	}else{
		return response()->json([
			'success' => false
		]);
	}
}
```

Setelah dinyatakan berhasil maka saya mempasing nik nya dan melakukan pemanggilan function table `getData`

```jsx
if(res.success){
	sessionStorage.setItem("eResepsionisNik", res.nik);
	getData(defaultFrom, defaultTo);
	// ...
}
```

### Tampilkan table
```javascript
function getData(fromDate, toDate) {
    $.ajax({
        type: "GET",
        url: "/e-resepsionis/berkas-masuk/getDataBerkasMasuk",
        data: {
            from: fromDate,
            to: toDate,
            username: sessionStorage.getItem("eResepsionisNik")
        },
        success: function(res) {
            $("#containerDocument").empty();

            if (res.length > 0) {
                res.forEach((item, index) => {
                    try {
                        $("#containerDocument").append(`
                            <tr>
                                <td>
                                    <center>${index + 1}</center>
                                </td>
                                <td>${item.name}</td>
                                <td>${item.username}</td>
                                <td>${item.departemen}</td>
                                <td>${item.description}</td>
                                <td>
                                    <center>
                                        <span class="btnStatus badge bg-primary text-white">${item.status}</span>
                                    </center>
                                </td>
                                <td>${moment(item.created_at).format('DD MMMM YYYY, HH:MM')}</td>
                                <td>
                                    <center>
                                        <button class="btn btn-sm btn-success" onClick="store(${item.id})">terima</button>
                                    </center>
                                </td>
                            </tr>
                        `);
                    } catch (error) {
                        console.error("Error parsing JSON data:", error);
                    }
                });
            } else {
                $("#containerDocument").append(`
                    <tr>
                        <td colspan="8" class="text-center">No records found</td>
                    </tr>
                `);
            }
        }
    });
}
```

Backendnya

```php
public function getDataBerkasMasuk(Request $req)
{
	$document = DB::table('document_progress')
				->where('document_progress.status','dibuat')
				->where('users.username', $req->username)
				->whereBetween('tanggal', [$req->from, $req->to])
				->join('users', 'document_progress.user_id','=','users.id')
				->join('departments', 'users.dept_id','=','departments.id')
				->select('users.name','users.username','document_progress.*','departments.name as departemen')
				->get();

	return response()->json($document);
}
```

Date: 01-08-2024