#Tech #Linux 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Install Apache|Install Apache]]
	- [[#Install Apache#Folder Apache Yg Harus Diketahui|Folder Apache Yg Harus Diketahui]]
- [[#Install MySQL|Install MySQL]]
- [[#Install PHP|Install PHP]]
	- [[#Install PHP#Ubah Datetime (Optional)|Ubah Datetime (Optional)]]
- [[#Final|Final]]

## Introduction
Kali ini gue berhasil mencoba buat setup development web (PHP, Apache,dan MySQL) tanpa xampp. Sebelumnya gue sudah berhasil setup pakai xampp di Linux, kali ini tanpa XAMPP. Langsung aja gue bahas!

Baca: [[Custom Domain di XAMPP Versi Linux]]

## Install Apache
Silahkan buka terminal (ctrl + alt + t) dan ketik perintah berikut:

```bash
sudo apt install apache2
```

### Folder Apache Yg Harus Diketahui
- **`/var/www/html`**: Ini adalah direktori default untuk file web. Lo dapat menempatkan file HTML, PHP, dan aplikasi web lainnya di sini.
- **`/etc/apache2/`**: Ini adalah direktori utama untuk semua file konfigurasi Apache. 
    - **`apache2.conf`**: File konfigurasi utama untuk Apache.
    - **`ports.conf`**: File yang mengonfigurasi port yang digunakan oleh Apache.
    - **`sites-available/`**: Folder ini berisi file konfigurasi untuk semua situs yang tersedia. Anda dapat membuat file konfigurasi baru di sini untuk virtual host.
    - **`sites-enabled/`**: Folder ini berisi symlink ke file konfigurasi di `sites-available/` yang diaktifkan. Anda dapat mengaktifkan situs dengan menggunakan perintah `a2ensite`.
    - **`mods-available/`**: Folder ini berisi modul-modul yang tersedia untuk Apache.
    - **`mods-enabled/`**: Folder ini berisi symlink ke modul-modul yang diaktifkan.
- **`/var/log/apache2/`**: Folder ini berisi file log untuk Apache, termasuk:
    - **`error.log`**: Log kesalahan yang terjadi pada server.
    - **`access.log`**: Log akses yang mencatat semua permintaan yang diterima oleh server.
- **`/etc/apache2/conf-available/`**: Folder ini berisi file konfigurasi tambahan yang dapat diaktifkan.
- **`/etc/apache2/conf-enabled/`**: Folder ini berisi symlink ke file konfigurasi tambahan yang diaktifkan.

## Install MySQL

```bash
sudo apt install mysql-server
```

Untuk mengecheck apakah mysql sudah dijalankan.

```bash
sudo systemctl status mysql
```

Note: silahkan install DBEaver untuk database manager agar kita lebih mudah manage database nya.

## Install PHP
Kebutuhan gue saat ini menggunakan php versi 7.4, mengikuti versi php di kantor (PT.PAS). Awal kita harus update terlebih dahulu.

```bash
sudo apt update
sudo apt install php7.4 php7.4-mysql php7.4-curl libapache2-mod-php7.4
```

Bila error kita gunakan repository milik ondrej

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```

### Ubah Datetime (Optional)
Ubah datetime kita bisa gunakan perintah berikut:

```bash
sudo nano /etc/php/7.4/apache2/php.ini
```

Lalu modifikasi kode berikut:

```bash
date.timezone = "Asia/Jakarta";
```

Jangan lupa restart apache2 nya dengan mengetik perintah `sudo systemctl restart apache2`. Untuk memastikan versi sudah sesuai kita cukup ketik perintah:

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
dan lihat localhost/info.php
```

Lalu buka browser dan kunjungi http://localhost/info.php

![[Screenshot from 2025-04-13 08-17-00.png]]

## Final
Terakhir kita bisa menaruh project kita di folder `var/www/html`. Bila terjadi error permission denied ketik perintah:

```bash
sudo chmod -R 777 var
```

Date: 13-04-2025