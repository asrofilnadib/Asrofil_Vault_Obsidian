#Tech 
# Table of Content
- [[#Jangan migrate:fresh|Jangan migrate:fresh]]

## Jangan migrate:fresh
Kejadian ketika saya ingin memperbarui 1 migrasi tanpa membuat migrasi baru. Saya sebelumnya sering banget migrate:fresh. Saya lupa bahwa database kerjaan saya ini banyak data. Alhamdulillahnya lagi di local. Jadi ngga berdampak ke server.

Jadi.... `php artisan migrate:fresh` dalam Laravel memang akan _menghapus_ semua tabel di database dan menjalankan ulang semua migrasi. Ini berarti _semua data dalam tabel akan dihapus_ dan _tabel akan dibuat ulang_

NOTE: Cocok untuk project awal yg tidak memiliki banyak data. Jika sudah banyak data JANGAN LUPA di backup terlebih dahulu.

Date: 11-07-2024