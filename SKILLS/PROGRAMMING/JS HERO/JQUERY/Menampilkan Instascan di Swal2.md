#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Screenshot nya|Screenshot nya]]
- [[#Kode nya|Kode nya]]

## Introduction
Sebelumnya gue pake instascan di dalam modal, nah sekarang bisa di dalam swal2.

## Screenshot nya
<img src="Screenshot_300.png" width="250" align="left">











## Kode nya
```html
<script src="{{ url('/assets/plugins/custom/qrcode/instascan.min.js') }}"></script>

function updateStatus(status) {
    Swal.fire({
        title: 'Scan QR Code',
        html: '<video id="preview" style="width: 100%;"></video>',
        showCancelButton: true,
        confirmButtonText: 'Close',
        onOpen: () => {
            const video = document.getElementById('preview');
            const scanner = new Instascan.Scanner({ video: video });

            scanner.addListener('scan', function (content) {
                Swal.fire({
                    title: 'QR Code Scanned!',
                    text: content,
                    icon: 'success'
                });
            });

            Instascan.Camera.getCameras().then(function (cameras) {
                if (cameras.length > 0) {
                    const cameraToUse = cameras.length > 1 ? cameras[1] : cameras[0];
                    scanner.start(cameraToUse);                
                } else {
                    Swal.fire('No cameras found.');
                }
            }).catch(function (e) {
                console.error(e);
                Swal.fire('Error accessing camera.');
            });
        },
        willClose: () => {
            const video = document.getElementById('preview');
            if (video.srcObject) {
                const stream = video.srcObject;
                const tracks = stream.getTracks();
                tracks.forEach(track => track.stop());
            }
        }
    });
}
```

Penjelasan:
- **`title`**: Menetapkan judul modal menjadi "Scan QR Code".
- **`html`**: Menyisipkan elemen video HTML dengan ID **`preview`**, yang akan digunakan untuk menampilkan umpan video dari kamera.
- **`showCancelButton`**: Menampilkan tombol "Cancel" di modal.
- **`confirmButtonText`**: Menetapkan teks tombol konfirmasi menjadi "Close".
- **`onOpen`**: Ini adalah callback yang dipanggil ketika modal dibuka. Di sini, kita akan menginisialisasi pemindaian QR code.
- **Mengambil Elemen Video**: Mengambil elemen video dengan ID **`preview`** untuk menampilkan umpan kamera.
- **Instascan.Scanner**: Membuat instance dari **`Instascan.Scanner`**, yang akan digunakan untuk memindai QR code. Parameter **`video`** diatur ke elemen video yang telah diambil.
- **`content`**: Ini adalah data yang dipindai dari QR code.
- **Menampilkan Hasil**: Menampilkan modal baru dengan judul "QR Code Scanned!" dan menampilkan isi dari QR code yang dipindai.
- **Jika Kamera Ditemukan**: Jika ada kamera yang ditemukan, pemindaian dimulai dengan kamera pertama (**`cameras[0]`**).
- **Jika Tidak Ada Kamera**: Menampilkan pesan "No cameras found." jika tidak ada kamera yang tersedia.
- **Error Handling**: Jika terjadi kesalahan saat mencoba mengakses kamera, kesalahan tersebut dicetak ke konsol dan menampilkan pesan "Error accessing camera."
- **Menghentikan Stream**: Jika elemen video memiliki **`srcObject`**, kita mengambil stream dan menghentikan semua track (kamera) untuk membebaskan sumber daya.

NOTE: Gunakan kode dibawah agar lebih fleksibel device. Maksudnya adalah bila lo buka di hp/tablet otomatis akan pakai kamera belakang. Bila menggunakan laptop langsung webcam kita yg dipakai.

```javascript
const cameraToUse = cameras.length > 1 ? cameras[1] : cameras[0];
scanner.start(cameraToUse);
```