#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Normalisasi|Normalisasi]]
- [[#1NF|1NF]]
- [[#2NF|2NF]]
- [[#3NF|3NF]]

## Introduction
Melanjutkan jurnaling [[Prinsip Desain Database]].

## Normalisasi
**Normalisasi** adalah proses menyusun data dalam tabel agar:
- Tidak ada duplikasi data.
- Data lebih rapi dan terstruktur.
- Mudah dikelola saat terjadi perubahan (update), penambahan (insert), atau penghapusan (delete).

Ada beberapa tahap normalisasi. Kita akan fokus ke:
- 1NF (First Normal Form).
- 2NF (Second Normal Form).
- 3NF (Third Normal Form).

## 1NF
Setiap kolom hanya boleh berisi **satu nilai tunggal** , tidak ada nilai yang "berkelompok" atau "ganda". Dibawah ini contoh table yg memiliki nilai ganda.

![[Pasted image 20250607110639.png]]

Dengan menerapkan 1NF maka tidak ada nilai ganda lagi. 

![[Pasted image 20250607110715.png]]

## 2NF
Dipecah menjadi 2 table. Terdapat Primary Key dan Foreign Key.

![[Pasted image 20250607110926.png]]

## 3NF
Semua kolom hanya bergantung pada primary key, tidak pada kolom lain.

![[Pasted image 20250607111428.png]]

**Improvement With Qwen AI**

Date: 07-06-2025