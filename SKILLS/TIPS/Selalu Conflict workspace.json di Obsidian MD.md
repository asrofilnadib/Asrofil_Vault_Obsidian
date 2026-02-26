#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#1. Hapus Cache Obsidian|1. Hapus Cache Obsidian]]
- [[#2. Jangan Lupa Commit & Push|2. Jangan Lupa Commit & Push]]

## Introduction
Ada case dimana gue selalu kena conflict git pull karena gue pake 2 laptop. Intinya gue lupa menambahkan `.gitignore`. Apa itu gitignore? File `.gitignore` adalah sebuah file yang digunakan dalam sistem kontrol versi Git untuk menentukan file atau direktori mana yang harus diabaikan oleh Git.

Laptop kantor dan laptop rumah.  Berikut errornya:

<img src="Pasted image 20250411082206.png" width="450" align="left">



Cara mengatasinya mudah:

## 1. Hapus Cache Obsidian
Jika folder `.obsidian` sudah dilacak sebelumnya, lo perlu menghapusnya dari indeks Git agar pengecualian ini berlaku. Gunakan perintah berikut:

```bash
git rm -r --cached .obsidian
```

## 2. Jangan Lupa Commit & Push

```bash
git add .gitignore
git commit -m "Menambahkan .obsidian ke .gitignore"
git push
```

Date: 11-04-2025