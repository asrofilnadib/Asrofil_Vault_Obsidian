#Tech #Linux 
# Table of Content
- [[#Introduction|Introduction]]
	- [[#Introduction#**Langkah 1: Mengaktifkan Modul SSL**|**Langkah 1: Mengaktifkan Modul SSL**]]
	- [[#Introduction#**Langkah 2: Membuat Sertifikat SSL**|**Langkah 2: Membuat Sertifikat SSL**]]
	- [[#Introduction#**Langkah 3: Konfigurasi Virtual Host untuk HTTPS**|**Langkah 3: Konfigurasi Virtual Host untuk HTTPS**]]
	- [[#Introduction#**Langkah 4: Restart XAMPP**|**Langkah 4: Restart XAMPP**]]
	- [[#Introduction#**Langkah 5: Akses HTTPS**|**Langkah 5: Akses HTTPS**]]
- [[#Contoh File httpd-ssl.conf|Contoh File httpd-ssl.conf]]
- [[#Screenshot|Screenshot]]

## Introduction
Melanjutkan dari seri sebelumnya [[Custom Domain di XAMPP Versi Linux]], kali ini gue mau install SSL. Kenapa sih install ssl? Karena ada plugin yg harus banget pake https. Misal Instascan.

### **Langkah 1: Mengaktifkan Modul SSL**
1. **Buka Terminal** dan jalankan perintah berikut untuk mengedit file konfigurasi Apache:
    
    ```bash
    sudo nano /opt/lampp/etc/httpd.conf
    ```
    
2. **Aktifkan Modul SSL**:
    - Cari baris yang berisi **`#LoadModule ssl_module modules/mod_ssl.so`** dan hapus tanda **`#`** di depannya untuk mengaktifkan modul SSL.
    - Pastikan juga ada baris berikut:
        
        ```
        Include etc/extra/httpd-ssl.conf
        ```
        
3. **Simpan dan Keluar** dari editor dengan menekan **`CTRL + O`**, kemudian **`Enter`**, dan **`CTRL + X`**.

### **Langkah 2: Membuat Sertifikat SSL**
1. **Buka Terminal** dan jalankan perintah berikut untuk membuat direktori untuk sertifikat SSL:
    
    ```bash
    sudo mkdir /opt/lampp/etc/ssl
    ```
    
2. **Buat Sertifikat SSL**: Jalankan perintah berikut untuk membuat sertifikat SSL dan kunci privat:
    
    ```bash
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /opt/lampp/etc/ssl/server.key -out /opt/lampp/etc/ssl/server.crt
    ```
    
    - Anda akan diminta untuk mengisi beberapa informasi. Anda dapat mengisi sesuai keinginan atau menekan **`Enter`** untuk melewati.

### **Langkah 3: Konfigurasi Virtual Host untuk HTTPS**
1. **Edit File Konfigurasi SSL**: Buka file konfigurasi SSL di XAMPP:
    
    ```bash
    sudo nano /opt/lampp/etc/extra/httpd-ssl.conf
    ```
    
2. **Tambahkan Virtual Host**: Temukan bagian yang berisi **`<VirtualHost _default_:443>`** dan ubah atau tambahkan konfigurasi berikut:
    
    ```
    <VirtualHost _default_:443>
        DocumentRoot "/opt/lampp/htdocs/pas/public"
        ServerName pas.test:443
    
        SSLEngine on
        SSLCertificateFile "/opt/lampp/etc/ssl/server.crt"
        SSLCertificateKeyFile "/opt/lampp/etc/ssl/server.key"
    
        <Directory "/opt/lampp/htdocs/pas/public">
            AllowOverride All
            Require all granted
        </Directory>
    
        ErrorLog "logs/pas.test-error.log"
        CustomLog "logs/pas.test-access.log" common
    </VirtualHost>
    ```
    

### **Langkah 4: Restart XAMPP**

Setelah melakukan semua konfigurasi, restart XAMPP untuk menerapkan perubahan:

```bash
sudo /opt/lampp/lampp restart
```

### **Langkah 5: Akses HTTPS**

Sekarang Anda dapat mengakses aplikasi Anda melalui HTTPS dengan mengetikkan **`https://pas.test`** di browser. Anda mungkin akan mendapatkan peringatan keamanan karena sertifikat yang ditandatangani sendiri. Anda dapat mengabaikan peringatan ini dan melanjutkan ke situs.

## Contoh File httpd-ssl.conf
/tech/tips/img/httpd-ssl.conf.txt

## Screenshot
![[pasang_ssl_linux.png]]

Tested on: Elementary OS