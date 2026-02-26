#Tech #Apache2 #Laravel #Ubuntu #SSL #AutoStart

# Table of contents
- [[#Introduction]]
- [[#Current Configuration Overview]]
- [[#Step-by-Step Setup]]
- [[#Enable Required Apache Modules]]
- [[#Configure ports.conf]]
- [[#Set Up HTTP (Port 8000)]]
- [[#Set Up HTTPS (SSL on Port 8000)]]
- [[#Enable Site Configurations]]
- [[#Test and Restart Apache]]
- [[#Verify Auto-Start on Boot]]
- [[#Troubleshooting Tips]]

## Introduction

Tujuan dari note ini adalah memastikan **Apache2 di Ubuntu secara otomatis menjalankan project Laravel Anda pada boot**, baik melalui **HTTP (port 8000)** maupun **HTTPS (SSL di port 8000)**, **tanpa perlu menjalankan `php artisan serve`**. Semua konfigurasi akan mengarah ke folder proyek Anda:

- HTTP & HTTPS: `/home/asrofil/Project/newpas-master/public`

Konfigurasi ini memanfaatkan **Apache Virtual Host** dan **SSL self-signed**, sehingga aplikasi Laravel bisa diakses via:
- `http://localhost:8000`
- `https://localhost:8000`

## Current Configuration Overview

Berdasarkan file yang diunggah:

- **`ports.conf`** → Mendengarkan port `8000` (HTTP dan SSL)
- **`000-default.conf`** → VirtualHost untuk HTTP di port `8000`
- **`default-ssl.conf`** → Berisi dua VirtualHost:
  - Satu di `*:443` (default, mengarah ke `/var/www/html`)
  - Satu di `*:8000` (SSL, mengarah ke `/home/asrofil/Project/newpas-master/public`)

Namun, untuk konsistensi dan keandalan, kita akan **membersihkan konfigurasi**, memastikan hanya satu lokasi proyek yang digunakan, dan mengaktifkan layanan secara permanen.

## Step-by-Step Setup

> ⚠️ Pastikan Apache2 sudah terinstal:

```bash
sudo apt install apache2
```

## Enable Required Apache Modules

Aktifkan modul yang diperlukan untuk `.htaccess` dan SSL:

```bash
sudo a2enmod rewrite ssl headers
```

## Configure ports.conf

Edit `/etc/apache2/ports.conf` agar hanya mendengarkan port yang dibutuhkan:

```bash
sudo nano /etc/apache2/ports.conf
```

Isi dengan:

```apache
Listen 80

<IfModule ssl_module>
    Listen 8000
    Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>
```

## Set Up HTTP (Port 8000)

Buat atau pastikan konfigurasi HTTP di `/etc/apache2/sites-available/000-default.conf`:

```apache
<VirtualHost *:8000>
    ServerAdmin webmaster@localhost
    ServerName localhost
    DocumentRoot /home/asrofil/Project/newpas-master/public

    <Directory /home/asrofil/Project/newpas-master/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

## Set Up HTTPS (SSL on Port 8000)

Buat file SSL terpisah agar rapi:  
`/etc/apache2/sites-available/default-ssl.conf`

```bash
sudo nano /etc/apache2/sites-available/default-ssl.conf
```

Isi dengan:

```bash
VirtualHost *:443>
        ServerAdmin webmaster@localhost

        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf

        #   SSL Engine Switch:
        #   Enable/Disable SSL for this virtual host.
        SSLEngine on

        #   A self-signed (snakeoil) certificate can be created by installing
        #   the ssl-cert package. See
        #   /usr/share/doc/apache2/README.Debian.gz for more info.
        #   If both key and certificate are stored in the same file, only the
        #   SSLCertificateFile directive is needed.
        SSLCertificateFile    /etc/ssl/localcerts/apache-selfsigned.crt
        SSLCertificateKeyFile /etc/ssl/localcerts/apache-selfsigned.key

        #   Server Certificate Chain:
        #   Point SSLCertificateChainFile at a file containing the
        #   concatenation of PEM encoded CA certificates which form the
        #   certificate chain for the server certificate. Alternatively
        #   the referenced file can be the same as SSLCertificateFile
        #   when the CA certificates are directly appended to the server
        #   certificate for convinience.
        #SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt

        #   Certificate Authority (CA):
        #   Set the CA certificate verification path where to find CA
        #   certificates for client authentication or alternatively one
        #   huge file containing all of them (file must be PEM encoded)
        #   Note: Inside SSLCACertificatePath you need hash symlinks
        #         to point to the certificate files. Use the provided
        #         Makefile to update the hash symlinks after changes.
        #SSLCACertificatePath /etc/ssl/certs/
        #SSLCACertificateFile /etc/apache2/ssl.crt/ca-bundle.crt

        #   Certificate Revocation Lists (CRL):
        #   Set the CA revocation path where to find CA CRLs for client
        #   authentication or alternatively one huge file containing all
        #   of them (file must be PEM encoded)
        #   Note: Inside SSLCARevocationPath you need hash symlinks
        #         to point to the certificate files. Use the provided
        #         Makefile to update the hash symlinks after changes.
        #SSLCARevocationPath /etc/apache2/ssl.crl/
        #SSLCARevocationFile /etc/apache2/ssl.crl/ca-bundle.crl

        #   Client Authentication (Type):
        #   Client certificate verification type and depth.  Types are
        #   none, optional, require and optional_no_ca.  Depth is a
        #   number which specifies how deeply to verify the certificate
        #   issuer chain before deciding the certificate is not valid.
        #SSLVerifyClient require
        #SSLVerifyDepth  10

        #   SSL Engine Options:
        #   Set various options for the SSL engine.
        #   o FakeBasicAuth:
        #    Translate the client X.509 into a Basic Authorisation.  This means that
        #    the standard Auth/DBMAuth methods can be used for access control.  The
        #    user name is the `one line' version of the client's X.509 certificate.
        #    Note that no password is obtained from the user. Every entry in the user
        #    file needs this password: `xxj31ZMTZzkVA'.
        #   o ExportCertData:
        #    This exports two additional environment variables: SSL_CLIENT_CERT and
        #    SSL_SERVER_CERT. These contain the PEM-encoded certificates of the
        #    server (always existing) and the client (only existing when client
        #    authentication is used). This can be used to import the certificates
        #    into CGI scripts.
        #   o StdEnvVars:
        #    This exports the standard SSL/TLS related `SSL_*' environment variables.
        #    Per default this exportation is switched off for performance reasons,
        #    because the extraction step is an expensive operation and is usually
        #    useless for serving static content. So one usually enables the
        #    exportation for CGI and SSI requests only.
        #   o OptRenegotiate:
        #    This enables optimized SSL connection renegotiation handling when SSL
        #    directives are used in per-directory context.
        #SSLOptions +FakeBasicAuth +ExportCertData +StrictRequire
        <FilesMatch "\.(?:cgi|shtml|phtml|php)$">
                SSLOptions +StdEnvVars
        </FilesMatch>
        <Directory /usr/lib/cgi-bin>
                SSLOptions +StdEnvVars
        </Directory>
        DocumentRoot /home/asrofil/Project/newpas-master/public
        <Directory /home/asrofil/Project/newpas-master/public>
                AllowOverride All
                Require all granted
        </Directory>
</VirtualHost>

<IfModule mod_ssl.c>
    <VirtualHost *:8000>
        ServerAdmin webmaster@localhost
        ServerName localhost
        DocumentRoot /home/asrofil/Project/newpas-master/public

        <Directory /home/asrofil/Project/newpas-master/public>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/ssl_error.log
        CustomLog ${APACHE_LOG_DIR}/ssl_access.log combined

        SSLEngine on
        SSLCertificateFile /etc/ssl/certs/ssl-cert-snakeoil.pem
        SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key

        <FilesMatch "\.(?:cgi|shtml|phtml|php)$">
            SSLOptions +StdEnvVars
        </FilesMatch>
    </VirtualHost>
</IfModule>
```

> ✅ Sertifikat self-signed (`snakeoil`) sudah tersedia secara default di Ubuntu. Jika belum:

```bash
sudo apt install ssl-cert
```

## Enable Site Configurations

Aktifkan kedua site:

```bash
sudo a2ensite 000-default.conf
sudo a2ensite default-ssl.conf
```

Jika file SSL Anda saat ini bernama `default-ssl.conf`, pastikan hanya bagian yang relevan (`*:8000`) yang aktif, atau ganti isinya seperti di atas.

## Test and Restart Apache

Uji konfigurasi:

```bash
sudo apache2ctl configtest
```

Jika muncul `Syntax OK`, restart Apache:

```bash
sudo systemctl restart apache2
```

## Verify Auto-Start on Boot

Apache2 di Ubuntu **secara default sudah diatur untuk start otomatis saat boot**. Verifikasi:

```bash
sudo systemctl is-enabled apache2
```

Jika output **`enabled`**, maka sistem Anda siap.

Jika belum:

```bash
sudo systemctl enable apache2
```

## Troubleshooting Tips

- **Akses ditolak?** Pastikan folder `/home/asrofil/Project/...` bisa dibaca oleh user `www-data`:
  ```bash
  sudo chmod -R 755 /home/asrofil/Project
  sudo chown -R www-data:www-data /home/asrofil/Project/newpas-master/storage /home/asrofil/Project/newpas-master/bootstrap/cache
  ```
- **Atau** pastikan folder `/home/asrofil/Project/...` dapat dibaca oleh apache2:
 ```bash
 sudo chown -R www-data:www-data /home/asrofil/Project/newpas-master/storage
sudo chown -R www-data:www-data /home/asrofil/Project/newpas-master/bootstrap/cache
 ```
- **SSL tidak jalan?** Pastikan modul `mod_ssl` aktif dan sertifikat ada.
- **Port 8000 tidak merespons?** Cek firewall:
  ```bash
  sudo ufw allow 8000
  ```
- **Masih diarahkan ke port 80?** Pastikan tidak ada konfigurasi lain yang listen di `*:80` dan mengarah ke lokasi berbeda.

Dengan langkah-langkah ini, **Laravel Anda akan selalu berjalan otomatis via Apache2 saat sistem boot**, tanpa perlu `php artisan serve`, dan siap diakses baik lewat HTTP maupun HTTPS di port `8000`.