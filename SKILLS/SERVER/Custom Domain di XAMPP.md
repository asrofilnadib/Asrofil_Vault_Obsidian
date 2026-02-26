#Tech 
# Table of Content
- [[#Introduction|Introduction]]
	- [[#Introduction#Update Windows Host file|Update Windows Host file]]
	- [[#Introduction#Update Apache virtual host|Update Apache virtual host]]

## Introduction
_Nama domain_ selalu mudah diingat dan juga memberikan tampilan profesional dalam pengembangan lokal daripada mengetikkan IP (127.0.0.1) atau localhost dan direktori file.

Secara default saat menggunakan server pengembangan lokal XAMPP, Anda harus mengetik `localhost/{directory_of_web_application}` atau `127.0.0.1/{directory_of_web_application}` di browser. Di XAMPP/WAMP, tidak ada metode satu klik langsung seperti di localWP untuk membuat domain khusus.

Tutorial ini akan membantu Anda membuat URL domain khusus yang mudah digunakan di WAMP dan XAMPP.

Katakanlah Anda memiliki aplikasi lokal di `localhost/mylocalapp`. Setelah mengikuti tutorial ini, Anda dapat mengakses aplikasi tersebut dengan mylocalapp.test atau nama domain apa pun yang Anda inginkan.

### Update Windows Host file
Sekarang, Perbarui file host Windows untuk memberi tahu DNS lokal untuk mengarahkan ulang domain Anda ke komputer lokal Anda (Komputer yang sama).

- Langkah 1: Buka `C:\Windows\System32\drivers\etc` dan edit file host "sebagai Administrator". Untuk itu buka, editor teks apa pun dalam mode administrator lalu arahkan ke file host.
- Langkah 2: Tambahkan 127.0.0.1 dan domain khusus di akhir file.

**C:/Windows/System32/drivers/etc**

```
127.0.0.1 mylocalapp.test
```

### Update Apache virtual host

Host virtual mengalihkan domain tertentu ke direktori file aplikasi tertentu di server.
- Langkah 1: Buka `C:\xampp\apache\conf\extra` dan buka file httpd-vhosts.conf.
- Langkah 2: Tambahkan konfigurasi host virtual untuk URL tertentu di akhir file.

**C:/xampp/apache/conf/extra**

```
<VirtualHost *:80>
    ServerName mylocalapp.test
    DocumentRoot C:/xampp/htdocs/mylocalapp/public
    DirectoryIndex index.php
</VirtualHost>
```

Terakhir save dan restart xampp nya.