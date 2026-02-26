![[Pasted image 20250407145040.png]]

#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Implementasi|Implementasi]]

## Introduction
Di project SILO ini gue mencoba untuk mengambil posisi gue saat ini di Plugin maps leafletJS.

## Implementasi
```javascript
var map = L.map('map').setView([-7.250445, 112.768845], 14);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '© OpenStreetMap'
}).addTo(map);

function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition, showError);
    } else {
        alert("Geolocation tidak didukung oleh browser ini.");
    }
}

function showPosition(position) {
    var lokasiAnda = [position.coords.latitude, position.coords.longitude];

    var customIcon = L.icon({
        iconUrl: '/simobil_silo/M01.png',
        iconSize: [50, 50],
        iconAnchor: [25, 50],
        popupAnchor: [0, -50]
    });

    var marker = L.marker(lokasiAnda, { icon: customIcon }).addTo(map)
        .bindPopup('M01')
        .openPopup();

    map.setView(lokasiAnda, 14);
}

function showError(error) {
    switch(error.code) {
        case error.PERMISSION_DENIED:
            alert("Pengguna menolak permintaan untuk mendapatkan lokasi.");
            break;
        case error.POSITION_UNAVAILABLE:
            alert("Lokasi tidak tersedia.");
            break;
        case error.TIMEOUT:
            alert("Permintaan untuk mendapatkan lokasi telah habis.");
            break;
        case error.UNKNOWN_ERROR:
            alert("Terjadi kesalahan yang tidak diketahui.");
            break;
    }
}

getLocation();
```

Penjelasan:
- Fungsi `getLocation` memeriksa apakah geolocation didukung oleh browser. Jika ya, ia mencoba mendapatkan posisi pengguna dengan `navigator.geolocation.getCurrentPosition`, yang memanggil `showPosition` jika berhasil dan `showError` jika gagal.
- Fungsi `showPosition` menerima objek `position` yang berisi informasi tentang lokasi pengguna. Koordinat latitude dan longitude disimpan dalam variabel `lokasiAnda`.
- Fungsi `showError` menangani berbagai kesalahan yang mungkin terjadi saat mencoba mendapatkan lokasi pengguna, memberikan pesan yang sesuai berdasarkan jenis kesalahan.

Date: 07-04-2025