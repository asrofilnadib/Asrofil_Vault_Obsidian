#Tech #Linux 
# Table of Content
- [[#**Langkah-langkah Mengatur Domain Kustom di XAMPP Linux**|**Langkah-langkah Mengatur Domain Kustom di XAMPP Linux**]]
- [[#Hasil|Hasil]]

### **Langkah-langkah Mengatur Domain Kustom di XAMPP Linux**
1. **Edit File Hosts**
    - Buka terminal dan edit file **`hosts`** dengan perintah berikut:
        
        ```bash
        sudo nano /etc/hosts
        ```
        
    - Tambahkan baris berikut di akhir file:
        
        ```bash
        127.0.0.1   pas.test
        ```
        
    - Simpan dan keluar dari editor dengan menekan **`CTRL + O`**, kemudian **`Enter`**, dan **`CTRL + X`**.
        
2. **Konfigurasi Virtual Host di XAMPP**
    - Anda perlu menambahkan konfigurasi virtual host di file ini. Buka file tersebut dengan perintah:
        
        ```bash
        sudo nano /opt/lampp/etc/httpd.conf
        ```
        
    - Cari bagian yang berisi **`#Include etc/extra/httpd-vhosts.conf`** dan hapus tanda **`#`** di depannya untuk mengaktifkan file konfigurasi virtual host.
        
3. **Edit File Konfigurasi Virtual Host**
    - Selanjutnya, buka file **`httpd-vhosts.conf`** yang terletak di:
        
        ```bash
        sudo nano /opt/lampp/etc/extra/httpd-vhosts.conf
        ```
        
    - Tambahkan konfigurasi virtual host berikut di akhir file:
        
        ```
        <VirtualHost *:80>
         ServerName pas.test
         DocumentRoot "/opt/lampp/htdocs/pas/public"
        
         <Directory "/opt/lampp/htdocs/pas/public">
           AllowOverride All
           Require all granted
         </Directory>
         
         ErrorLog "logs/pas.test-error.log"
         CustomLog "logs/pas.test-access.log" common
        </VirtualHost>
        
        ```
        
    - Gantilah **`"/opt/lampp/htdocs/pas"`** dengan path yang sesuai ke direktori proyek Anda.
        
4. **Restart XAMPP**
    - Setelah melakukan perubahan, restart XAMPP untuk menerapkan konfigurasi baru:
        
        ```bash
        sudo /opt/lampp/lampp restart
        ```
        
5. **Akses Domain Kustom**
    - Sekarang Anda dapat mengakses proyek Anda melalui browser dengan mengetikkan **`http://pas.test`**.

## Hasil
![[custom_domain_xampp_linux.png]]

Oke aman ngga ada masalah!

Tested on: Elementary OS