#Tech #Linux 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Sertifikat SSL|Sertifikat SSL]]
- [[#Konfigurasi|Konfigurasi]]
- [[#Final|Final]]

## Introduction
Sebelumnya gue sudah berhasil [[Pasang SSL di XAMPP Versi Linux]], Kali ini gue berhasil install SSL tanpa XAMPP. Menurut gue sangat mudah dan simple ya. Berikut Caranya!

## Sertifikat SSL
Buat sertifikasi SSL dengan perintah berikut:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/pas.test.key -out /etc/ssl/certs/pas.test.crt
```

nama file `pas.test.key` mohon disesuaikan dengan project lo ya.

## Konfigurasi
ketik perintah berikut:

Dan tambahkan kode dibawah:

```
<VirtualHost *:443>
    ServerName pas.test
    DocumentRoot /path/to/your/laravel/public

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/pas.test.crt
    SSLCertificateKeyFile /etc/ssl/private/pas.test.key

    <Directory /path/to/your/laravel/public>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/pas.test-error.log
    CustomLog ${APACHE_LOG_DIR}/pas.test-access.log combined
</VirtualHost>
```

Konfigurasi sebelumya jangan dihapus ya. Hasilnya akan seperti gambar dibawah.

![[Pasted image 20250413135654.png]]

Baca: [[Custom Domain Tanpa Xampp Versi Linux]]

## Final
Ketik perintah berikut untuk finishing

```bash
sudo a2enmod ssl
sudo a2ensite pas.test.conf
sudo systemctl restart apache2
```

Oke kita berhasil memasang ssl.

![[Screenshot from 2025-04-13 13-58-50.png]]

Date: 13-04-2025