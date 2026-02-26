![[InstaScan PAS.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Lesgo...|Lesgo...]]
- [[#Logic JSnya|Logic JSnya]]
- [[#Controllernya|Controllernya]]

## Introduction
Kali ini saya disuruh untuk membuat clone aplikasi yg dibuat oleh mas Daniar dari Noodle2 menjadi Noodle1 intinya. Ada beberapa hal yg menarik menurut saya harus di buat catatan agar tidak lupa.

System ini namanya LP Packing. Intinya ada fitur yg diharuskan untuk scan barcode tapi melalui webcam atau kamera. Ini menggunakan Instascan, [selengkapnya](https://github.com/schmich/instascan)

## Lesgo...
```html
<div class="col-4">
	<button class="MulaiScan btn btn-dark">Mulai Scan</button>
	<button class="StopCamera hide btn btn-primary">Tutup Kamera</button>
</div>
<div class="col-6 col-sm-12">
	<div class="card">
		<div class="card-body hide cardscan text-center">
			<video id="reader" style="width: 400px; margin-left: auto; border-radius: 10px;"></video>
		</div>
	</div>
</div>
```

Kita buat 2 tombol dan 1 video untuk kita scan qr nya.

## Logic JSnya
```jsx
<script src="{{ url('/assets/plugins/custom/qrcode/instascan.min.js') }}"></script>

let scanner = new Instascan.Scanner({
	video: document.getElementById('reader'),
});
```

Pertama kita load js nya dan buat variable, panggil objek Instascan. minimal menggunakan property video. [Selengkapnya](https://github.com/schmich/instascan?tab=readme-ov-file#let-scanner--new-instascanscanneropts)

```javascript
scanner.addListener('scan', function (nik_login){
	var shift = $('#shift').val();
	var line = $('#line').val();
	var url = '{{ url('/prn2-lp/login') }}';

	if(shift == null || line == null){
		toastr.error("Shift dan Line Tidak Boleh Kosong !");
		return false;
	}

	$.ajax({
		type : "POST",
		dataType : "json",
		url : url,
		data : {
			nik_login : nik_login,
			shift : shift,
			line : line
		},
		success: function(val) {
			if (val["status"] == false) {
				errMsg(val['info']);
			}else{
				successMsg(val['info']);
				setTimeout(function() {
					window.location = '{{ url('/prn2-lp/transaction') }}';
				}, 1700);
			}
		},
		error: function(val) {
			console.log('Error: ', val);
		}
	});
});
```

`scanner.addListener('scan', function (nik_login){` perllu diperhatikan bahwa Instascan cara menangkap data hasil scan QrCodenya dengan menaruh parameter di function nya.

```javascript
Instascan.Camera.getCameras().then(function (cameras) {
   $('.MulaiScan').on('click', function(){
		var shift = $('#shift').val();
		var line = $('#line').val();
		if(shift == null || line == null){
			toastr.error("Shift dan Line Tidak Boleh Kosong !");
			return false;
		}
		scanner.start(cameras[0]);
		$('.MulaiScan').hide('slow');
		$('.StopCamera').show('slow');
		$('.cardscan').show('slow');
		$('#reader').show('slow');
	});
	$('.StopCamera').on('click', function(){
		scanner.stop();
		$('.MulaiScan').show('slow');
		$('.StopCamera').hide('slow');
		$('.cardscan').hide('slow');
		$('#reader').hide('slow');
	});
}).catch(function (e) {
	console.error(e);
});
```

Kode diatas untuk menampilkan kamera nya. `Instascan.Camera.getCameras().then(function (cameras) {` parameter function nya untuk pemanggilan kamera nya. Berikut full code nya...

```jsx
@push('scripts')
    <script src="{{ url('/assets/plugins/custom/qrcode/instascan.min.js') }}"></script>
    <script type="text/javascript">
        //Error Message Function
        function errMsg(msg){
            Swal.fire({
                icon: 'error',
                title: msg,
            })
        }

        //Success Message Function
        function successMsg(msg){
            Swal.fire({
                icon: 'success',
                title: msg,
                showConfirmButton: false,
                timer: 1500
            })
        }
        // $('select').select2();

        let scanner = new Instascan.Scanner({
            video: document.getElementById('reader'),
        });
        scanner.addListener('scan', function (nik_login){
            var shift = $('#shift').val();
            var line = $('#line').val();
            var url = '{{ url('/prn2-lp/login') }}';
            if(shift == null || line == null){
                toastr.error("Shift dan Line Tidak Boleh Kosong !");
                return false;
            }
            $.ajax({
                type : "POST",
                dataType : "json",
                url : url,
                data : {
                    nik_login : nik_login,
                    shift : shift,
                    line : line
                },
                success: function(val) {
                    if (val["status"] == false) {
                        errMsg(val['info']);
                    }else{
                        successMsg(val['info']);
                        setTimeout(function() {
                            window.location = '{{ url('/prn2-lp/transaction') }}';
                        }, 1700);
                    }
                },
                error: function(val) {
                    console.log('Error: ', val);
                }
            });
        });

        Instascan.Camera.getCameras().then(function (cameras) {
           $('.MulaiScan').on('click', function(){
                var shift = $('#shift').val();
                var line = $('#line').val();
                if(shift == null || line == null){
                    toastr.error("Shift dan Line Tidak Boleh Kosong !");
                    return false;
                }
                scanner.start(cameras[0]);
                $('.MulaiScan').hide('slow');
                $('.StopCamera').show('slow');
                $('.cardscan').show('slow');
                $('#reader').show('slow');
            });
            $('.StopCamera').on('click', function(){
                scanner.stop();
                $('.MulaiScan').show('slow');
                $('.StopCamera').hide('slow');
                $('.cardscan').hide('slow');
                $('#reader').hide('slow');
            });
      }).catch(function (e) {
        console.error(e);
      });
    </script>
@endpush
```

## Controllernya
```php
public function login(Request $request){
	$nik = DB::table('hr_karyawan')->where('nik','=',$request->nik_login)->get();
	$nama = '';
	if (count($nik) > 0) {
		foreach ($nik as $item) {
			$nama = $item->nama;
		}
		//Set Session
		session()->put('nik_transaction', $request->nik_login);
		session()->put('nama_transaction', $nama);
		session()->put('shift', $request->shift);
		session()->put('line', $request->line);
		return response()->json([
			'status' => true,
			'info' => 'Berhasil Login.'
		], 201);
	}else{
		return response()->json([
			'status' => false,
			'info' => $request->nik_login.' : NIK tidak ditemukan.'
		], 201);
	}
}
```

keluaran dari instascan itu 0 dan 1. That's it!

Date : 28-06-2024