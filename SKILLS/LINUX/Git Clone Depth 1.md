#Linux 
# Table of Content
- [[#Introduction|Introduction]]
- [[#git clone --depth 1|git clone --depth 1]]
- [[#Apakah mempengaruhi size file originalnya?|Apakah mempengaruhi size file originalnya?]]

## Introduction
Ada case dimana repository gue udah 1gb dengan banyak commit, pas di clone malah error karena terlalu besar size dan jaringan kurang stabil. Setelah gue mencari tahu, dapatlah gue 1 command yg powerful buat ngatasin masalah ini.

## git clone --depth 1
Perintah ini adalah cara untuk mengambil 1 commit paling akhir (bisa disesuaikan dengan angka nya).

- **Kelebihan:**
    - **Lebih cepat** karena ukuran file yang diunduh lebih kecil.
    - Menghemat **bandwidth** dan **penyimpanan lokal**.
    - Cocok saat kamu hanya ingin melihat atau memakai kode, **bukan histori commit-nya**.
- **Kekurangan:**
    - Tidak bisa melihat commit sebelumnya.
    - Tidak bisa melakukan `git log` secara lengkap.
    - Lebih terbatas saat ingin **rebase** atau **merge** dari commit lama.

## Apakah mempengaruhi size file originalnya?
**Tidak**, perintah `git clone --depth 1` **tidak mempengaruhi ukuran repository asli (original repository)** yang berada di server (misalnya di GitHub, GitLab, dll).

Date: 16-10-2025