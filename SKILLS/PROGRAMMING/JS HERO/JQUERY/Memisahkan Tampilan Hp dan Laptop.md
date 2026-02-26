#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Implementasi|Implementasi]]

## Introduction
Waktu itu saya membuat salah satu project request dari Manager Mie Sedaap. Di segi laravel sangat mudah untuk memisahkan logic yg menggunakan laptop dengan yg menggunakan android.

Bagaimana bila dari segi frontend? di Javascript nya? di CSS mudah lah ya untuk itu tinggal max-width saja. Berikut cara sederhana nya...

## Implementasi
```javascript
function autoCamera(no_sdp_scan){
    scanner = new Instascan.Scanner({
        video: document.getElementById('camera'),
    });

    scanner.addListener('scan', function (no_sdp){
        loading();
        const transaction = db.transaction("SdpTransaksiSuhu", "readonly");
        const sdpTransaksiStore = transaction.objectStore("SdpTransaksiSuhu");

        sdpTransaksiStore.getAll().onsuccess = function(event) {
            const data = event.target.result;
            const found = data.find(item => item.no_sdp === no_sdp);

            if (no_sdp == no_sdp_scan) {
                scanner.stop();
                Swal.close();
                processScanOffline(no_sdp);
            } else {
                Swal.fire({
                    title: "Error",
                    icon: "error",
                    text: "No SDP tidak sesuai!"
                });
            }
        };
    });

    Instascan.Camera.getCameras().then(function (cameras) {
        if (window.matchMedia("(max-width: 768px)").matches) {
            scanner.start(cameras[1]);
        }else{
            scanner.start(cameras[0]);
        }

		$('#camera').show('slow');
    });
}
```

Penjelasan:
- `window.matchMedia("(max-width: 768px)").matches` adalah sebuah perintah dalam JavaScript yang digunakan untuk memeriksa apakah lebar jendela tampilan (viewport) saat ini kurang dari atau sama dengan 768 piksel.
- Jika maks width 768 maka termasuk untuk ukuran handphone.

Note: Sudah di test di handphone dan laptop ya...

Date: 31-10-2024