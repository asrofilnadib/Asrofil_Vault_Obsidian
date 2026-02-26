![[Pasted image 20250422203238.png]]
#Tech 
# Table of Content

## Introduction
Kali ini gue develop portal bisnis punya temen gue dimana gue menggunakan Laravel 12. Sebelumnya gue menggunakan laravel 7 dimana kalau ada masalah csrf token intinya ke file berikut `app/http/middleware/VerifyCsrfToken.php`. Di Laravel 12 uniknya kita ngga ada file tersebut.

Best Practice nya kita harus menggunakan proteksi X-CSRF. Apa itu?

**X-CSRF-Token** adalah header khusus yang digunakan untuk menyertakan **CSRF token** dalam request HTTP, terutama untuk request AJAX. Token ini adalah bagian dari mekanisme keamanan Laravel (dan framework lainnya) untuk melindungi aplikasi web dari serangan **Cross-Site Request Forgery (CSRF)** .

CSRF adalah serangan keamanan di mana penyerang mencoba memanipulasi pengguna yang sudah diautentikasi untuk melakukan tindakan tidak disengaja di aplikasi web.

## Solusi
![[Pasted image 20250422204230.png]]


Kurang lebih seperti itu.

Date: 22-04-2025