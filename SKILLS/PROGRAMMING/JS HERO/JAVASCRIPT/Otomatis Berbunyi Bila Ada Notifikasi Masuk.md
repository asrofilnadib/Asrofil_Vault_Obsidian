#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Implementasinya|Implementasinya]]
	- [[#Implementasinya#Penjelasan:|Penjelasan:]]

## Introduction
Saya sedang mengerjakan project seperti gojek tapi versi lingkup kecil. Nama Projectnya **TMS (Transportation Management System).** Dan ada case dimana ketika notifikasi nya muncul, baiknya hp kita berbunyi selayaknya kurir sedang dapat orderan. Awalnya saya agak pesimis, tapi saya cari tahu ternyata bisa 🙂 Mau handphone nya ngga nyala tetap kalau ada notifikasi berbunyi. Tapi sebelumnya harus di allow ya sound nya.

Silahkan baca kembali penggunaan websocket. Karena materi tersebut saling terhubung dengan materi ini.

![[bandicam 2025-01-04 14-11-28-502.mp4]]

Note: berjalan dengan baik di mozilla. Silahkan izinkan penggunaan sound di permission browser ya.
## Implementasinya
```html
@push('scripts')
<script src="{{ asset('js/laravel-echo.js') }}"></script>
<script>
let audio = new Audio('/tms/notif_in.mp3'); 
let soundEnabled = false;

Notification.requestPermission().then(permission => {
    if (permission === 'granted') {
        console.log('Izin notifikasi diberikan.');
    }
});

document.addEventListener('click', function() {
    if (!soundEnabled) {
        audio.play().catch(error => {
            console.error('Error saat memutar suara:', error);
        });
    }
});

function playNotificationSound() {
    soundEnabled = true; 

    if (soundEnabled) {
        audio.currentTime = 0; // Reset audio ke awal
        audio.play().catch(error => {
            console.error('Error saat memutar suara:', error);
        });
    }
}

Echo.channel('NotifDriverIn').listen('.notif-driver-in', (data) => {
    playNotificationSound();
		// Kode lain...    
});
</script>
@endpush
```

### Penjelasan:
- Terdapat objek **`Audio`** yang dibuat untuk memuat file audio (**`/tms/notif_in.mp3`**).
- Suara hanya akan diputar setelah pengguna berinteraksi dengan halaman (misalnya, dengan mengklik halaman). Ini adalah langkah yang diambil untuk mematuhi kebijakan browser modern yang membatasi autoplay audio tanpa interaksi pengguna.
- Variabel **`soundEnabled`** digunakan untuk melacak apakah suara sudah diizinkan untuk diputar. Ketika pengguna mengklik halaman, suara akan diputar, dan **`soundEnabled`** diatur ke **`true`**.
- Fungsi **`playNotificationSound()`** digunakan untuk memutar suara notifikasi setiap kali notifikasi baru diterima.

`Note: Tested di Mozilla Firefox.`

Date: 04-01-2024