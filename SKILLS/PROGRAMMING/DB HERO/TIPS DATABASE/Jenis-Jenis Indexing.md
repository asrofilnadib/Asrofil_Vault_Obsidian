#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Apa Itu Indexing?|Apa Itu Indexing?]]
- [[#Tujuan Penggunaan Index|Tujuan Penggunaan Index]]
- [[#Jenis-Jenis Index Beserta Penggunaan|Jenis-Jenis Index Beserta Penggunaan]]
	- [[#Jenis-Jenis Index Beserta Penggunaan#1. Single Column Index|1. Single Column Index]]
	- [[#Jenis-Jenis Index Beserta Penggunaan#2. Composite Index (Gabungan Kolom)|2. Composite Index (Gabungan Kolom)]]
	- [[#Jenis-Jenis Index Beserta Penggunaan#3. Unique Index|3. Unique Index]]
	- [[#Jenis-Jenis Index Beserta Penggunaan#4. Primary Key Index|4. Primary Key Index]]
	- [[#Jenis-Jenis Index Beserta Penggunaan#5. Full-text Index|5. Full-text Index]]
- [[#Tips Menggunakan Index|Tips Menggunakan Index]]

## Introduction
Lanjutan dari jurnaling [[Prinsip Desain Database]].

## Apa Itu Indexing?
**Index** adalah struktur data khusus yang dibuat untuk mempercepat pencarian dan akses data dalam tabel. Mirip seperti indeks buku: kamu tidak perlu membaca seluruh isi buku untuk menemukan halaman tertentu — cukup lihat indeksnya saja.

## Tujuan Penggunaan Index
- Mempercepat operasi `SELECT` dan `WHERE`
- Membantu proses `JOIN` antar tabel
- Mempercepat pengurutan (`ORDER BY`)
- Meningkatkan performa aplikasi yang bekerja dengan data besar

## Jenis-Jenis Index Beserta Penggunaan
### 1. Single Column Index
- **Deskripsi:** Dibuat berdasarkan satu kolom.
- Kolom yang sering digunakan sebagai kondisi `WHERE`.
- Contoh: `email`, `username`, `id`.
```sql
CREATE INDEX idx_email ON users(email);
```        

### 2. Composite Index (Gabungan Kolom)
- **Deskripsi:** Dibuat dari dua atau lebih kolom.
- Untuk query dengan beberapa kondisi gabungan.
```sql
CREATE INDEX idx_name ON users(first_name, last_name);
```

### 3. Unique Index
- **Deskripsi:** Memastikan semua nilai dalam kolom unik.
- Kolom seperti `email`, `username`, `NIK`.
- Mencegah duplikasi data.
```sql
CREATE UNIQUE INDEX idx_username ON users(username);
```        
  
### 4. Primary Key Index
- **Deskripsi:** Otomatis dibuat saat menetapkan primary key.
- Setiap tabel harus punya satu primary key.
- Biasanya otomatis terindeks.
```sql
CREATE TABLE users (
	user_id INT PRIMARY KEY,
	name VARCHAR(100)
);
```        

### 5. Full-text Index
- **Deskripsi:** Untuk pencarian teks panjang menggunakan `MATCH ... AGAINST`.
- Kolom deskripsi, artikel, log.
- Cocok untuk pencarian kata kunci.
```sql
CREATE FULLTEXT INDEX idx_description ON products(description);
```

## Tips Menggunakan Index
- Gunakan index pada kolom yang sering muncul di:
    - `WHERE`
    - `JOIN`
    - `ORDER BY`
- Hindari over-indexing karena bisa memperlambat operasi `INSERT/UPDATE/DELETE`.
- Gunakan `EXPLAIN` untuk memeriksa apakah query sudah menggunakan index.
- Prioritaskan kolom dengan selektivitas tinggi (banyak nilai unik).
- Pastikan struktur index sesuai dengan pola query aplikasi.

**Improvement With Qwen AI**

Date: 07-06-2025