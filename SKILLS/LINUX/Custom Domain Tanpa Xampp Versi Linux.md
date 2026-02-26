#Tech #Linux 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Add Host|Add Host]]
- [[#Add Virtual Host|Add Virtual Host]]
- [[#Final|Final]]

## Introduction
Kali ini gue berhasil mencoba custom domain localhost:8000 ke domain.test. Dan tanpa kita menjalankan `php artisan serve`. Lebih hemat waktu juga hehe. Menurut gue cara nya lebih mudah dari kita pakai XAMPP.

Baca: [[Custom Domain di XAMPP Versi Linux]]

## Add Host
Ketik perintah berikut:

```bash
sudo nano /etc/hosts
```

Lalu tambahkan kode ini:

```
127.0.0.1   domain.test
```

## Add Virtual Host
VirtualHost pada port 80 digunakan untuk mengonfigurasi server web Apache agar dapat melayani permintaan HTTP untuk satu atau lebih domain tertentu.

Modifikasi file `domain.test.conf` dengan mengetik perintah berikut:

```bash
sudo nano /etc/apache2/sites-available/domain.test.conf
```

Lalu tambahkan konfigurasi berikut:

```
<VirtualHost *:80>
    ServerName domain.test
    DocumentRoot /path/to/your/laravel/public

    <Directory /path/to/your/laravel/public>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/domain.test-error.log
    CustomLog ${APACHE_LOG_DIR}/domain.test-access.log combined
</VirtualHost>
```

Path nya disesuaikan dengan project lo ya.

## Final
Ketik perintah dibawah:

```bash
sudo a2ensite domain.test.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

Penjelasan: 
1. **`sudo a2ensite domain.test.conf`**: Perintah ini mengaktifkan konfigurasi situs yang ditentukan dalam file `domain.test.conf`. Dengan menjalankan perintah ini, Apache akan mulai melayani permintaan untuk domain yang terdaftar dalam file konfigurasi tersebut. Secara default, situs yang baru dibuat tidak diaktifkan, sehingga perintah ini diperlukan untuk mengaktifkannya.
2. **`sudo a2enmod rewrite`**: Perintah ini mengaktifkan modul `rewrite` di Apache. Modul ini memungkinkan penggunaan aturan penulisan ulang URL, yang sering digunakan dalam aplikasi web untuk membuat URL yang lebih bersih dan ramah pengguna. Misalnya, ini penting untuk aplikasi Laravel agar dapat menangani permintaan dengan benar dan mengarahkan ke rute yang sesuai.
3. **`sudo systemctl restart apache2`**: Perintah ini me-restart layanan Apache. Setelah mengaktifkan situs dan modul, restart diperlukan agar perubahan konfigurasi yang baru diterapkan dan agar Apache mulai melayani permintaan sesuai dengan pengaturan yang telah diperbarui.

![[Screenshot from 2025-04-13 13-36-43.png]]

Date: 13-04-2025