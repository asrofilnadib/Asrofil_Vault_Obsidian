#Tech 
## Introduction 
Untuk memungkinkan pengguna **memilih gambar menggunakan foto langsung dengan kamera** saat mengunggah file (upload foto), lo perlu menambahkan atribut `capture` pada input file HTML.

Namun, penting diketahui bahwa dukungan terhadap fitur ini tergantung pada browser dan perangkat pengguna.

Teste: Tablet Samsung, Old Chrome.

`<input type="file" class="form-control" accept="image/*" capture="environment">`

Penjelasan: 
- `accept="image/*"`: Hanya izinkan file gambar.
- `capture="environment"`: Gunakan kamera belakang (jika tersedia).
- `capture="user"`: Gunakan kamera depan.
- Tanpa `capture`: Default akan membuka galeri/file manager.

![[WhatsApp Video 2025-09-06 at 12.05.23.mp4]]
Date: 06-09-2025