#Tech 
# Table of content

- [[#Introduction]]
- [[#Step-by-Step Solution: Fix Write Permission for PHP in Ubuntu 24.04]]
- [[#Why This Happens]]
- [[#Recommended Approach]]
- [[#Alternative Approaches (Use with Caution)]]
- [[#Verification]]
- [[#Important Notes]]

---
## Introduction

Aplikasi Laravel Anda gagal menulis file ke direktori `public/qc-premix-online/foto_kemasan` meskipun folder sudah ada dan `php artisan storage:link` sudah dijalankan. Error ini terjadi karena **web server (Apache/Nginx) berjalan sebagai user `www-data`, bukan user Anda (`asrofil`)**, sehingga tidak memiliki izin menulis ke folder tersebut. Solusi ini menjelaskan cara memperbaiki izin akses di Ubuntu 24.04 secara aman dan efektif.

---

## Step-by-Step Solution: Fix Write Permission for PHP in Ubuntu 24.04

### 1. Pastikan Direktori Tujuan Ada

Buka terminal dan pastikan struktur folder `foto_kemasan` benar-benar ada:

```bash
cd /home/asrofil/Project/newpas-master
mkdir -p public/qc-premix-online/foto_kemasan
```

> 💡 `mkdir -p` aman digunakan — tidak akan error jika folder sudah ada.

### 2. Ubah Ownership ke User Web Server

Ubah kepemilikan folder `qc-premix-online` dan semua isinya agar dimiliki oleh user `www-data` (user default web server di Ubuntu):

```bash
sudo chown -R www-data:www-data public/qc-premix-online/
```

> ✅ `-R` = rekursif (menerapkan ke semua subfolder dan file)  
> ✅ `www-data:www-data` = owner:group

### 3. Atur Permission Folder agar Bisa Ditulis

Berikan izin baca, tulis, dan eksekusi (masuk ke folder) untuk owner, dan baca-eksekusi untuk grup & lainnya:

```bash
sudo chmod -R 755 public/qc-premix-online/
```

> 🔍 Penjelasan mode `755`:
> 
> - Owner (`www-data`): `rwx` (baca, tulis, eksekusi)
> - Group & Others: `r-x` (hanya baca dan eksekusi)  
>     → Ini aman dan cukup untuk web server menulis file baru.

---

## Why This Happens

- Laravel (dan PHP) dijalankan oleh **web server** (Apache/Nginx), bukan oleh user Anda.
- Web server biasanya berjalan sebagai user `www-data`.
- Folder yang Anda buat secara manual (`public/qc-premix-online/foto_kemasan`) masih dimiliki oleh `asrofil`.
- Web server **tidak bisa menulis** ke folder yang dimiliki user lain tanpa izin eksplisit.
- `php artisan storage:link` hanya membuat symlink — **tidak mengubah izin folder custom**.

---

## Recommended Approach

✅ **Gunakan Solusi Langkah 1–3 di atas**  
Ini adalah pendekatan **paling aman dan standar** untuk lingkungan produksi maupun development:

|||
|---|---|
|Pastikan folder ada|`mkdir -p public/qc-premix-online/foto_kemasan`|
|Beri kepemilikan ke web server|`sudo chown -R www-data:www-data public/qc-premix-online/`|
|Beri izin tulis|`sudo chmod -R 755 public/qc-premix-online/`|

> ✅ Tidak perlu restart server — perubahan langsung berlaku.

---

## Alternative Approaches (Use with Caution)

### A. Tambahkan User Anda ke Grup `www-data` _(Development Only)_

Jika Anda sering mengedit file secara manual dan ingin tidak perlu `sudo`:

```bash
sudo usermod -a -G www-data asrofil
sudo chgrp -R www-data public/qc-premix-online/
sudo chmod -R 775 public/qc-premix-online/
```

> ⚠️ **Hanya untuk development** — memberi akses write ke grup web server bisa berisiko jika server diakses publik.

### B. `chmod 777` — JANGAN DIGUNAKAN!

```bash
sudo chmod -R 777 public/qc-premix-online/foto_kemasan
```

> ❌ **Sangat berbahaya** — membuka akses penuh ke semua pengguna. Rentan terhadap serangan eksploitasi.

---

## Verification

Setelah menerapkan solusi:

1. Coba upload file kembali melalui aplikasi web Anda.
2. Jika berhasil, file akan muncul di:
    
    /home/asrofil/Project/newpas-master/public/qc-premix-online/foto_kemasan/
    
3. Cek kepemilikan file yang baru diupload:
    ```bash
    ls -la public/qc-premix-online/foto_kemasan/
    ```
    
    → Harus terlihat owner `www-data`.

---

## Important Notes

- **Jangan pernah gunakan `chmod 777`** di lingkungan produksi.
- Jika Anda berganti web server (misal: dari Apache ke Nginx), pastikan user web servernya tetap `www-data`. Cek dengan:
    ```bash
    ps aux | grep nginx
    # atau
    
    ps aux | grep apache
    
    ```

- Jika folder `foto_kemasan` berada di luar `public`, pertimbangkan untuk memindahkannya ke dalam `storage/app/public` dan gunakan `storage:link` — ini lebih sesuai dengan arsitektur Laravel.
- Setelah perubahan izin, **refresh browser Anda** (bukan hanya reload), karena beberapa cache bisa menyimpan error lama.