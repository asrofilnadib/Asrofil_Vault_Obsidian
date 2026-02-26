#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#1. Normalisasi (Normalization)|1. Normalisasi (Normalization)]]
- [[#2. Integritas Referensial (Referential Integrity)|2. Integritas Referensial (Referential Integrity)]]
- [[#3. Pemilihan Tipe Data yang Tepat|3. Pemilihan Tipe Data yang Tepat]]
- [[#4. Penggunaan Primary Key|4. Penggunaan Primary Key]]
- [[#5. Indexing pada Kolom yang Sering Diquery|5. Indexing pada Kolom yang Sering Diquery]]
- [[#6. Beri Nama Kolom & Tabel Secara Konsisten|6. Beri Nama Kolom & Tabel Secara Konsisten]]
- [[#Penutupan|Penutupan]]

## Introduction
Jurnal ini dibuat karena ada beberapa system yg gue buat di PT. PAS dimana punya kesalahan besar di struktur database. Seperti redundancy, inkonsisten penamaan kolom (pake inggris, indo campur haha). Langsung aja berikut adalah beberapa **contoh prinsip desain database** yang penting untuk diterapkan agar menghasilkan struktur database yang efisien, konsisten, dan mudah dikelola:

## 1. Normalisasi (Normalization)
**Normalisasi (Normalization)** adalah proses mengorganisir data dalam tabel-tabel relasional untuk meminimalkan **redundansi data** dan **ketergantungan data** , sehingga meningkatkan **integritas data** dan **efisiensi penyimpanan** .

Tujuan utamanya adalah:
- Menghindari duplikasi data yang tidak perlu.
- Menjaga konsistensi data saat terjadi perubahan (update).
- Mencegah anomali seperti _insert anomaly_ , _update anomaly_ , dan _delete anomaly_ .

![[Pasted image 20250607095926.png]]

Baca: [[Mengenal 1NF hingga 3NF]]

## 2. Integritas Referensial (Referential Integrity)
**Integritas referensial** adalah aturan dalam database relasional yang memastikan bahwa **hubungan antar-tabel tetap konsisten** . Artinya, jika sebuah tabel memiliki kolom yang merujuk ke data di tabel lain (melalui _foreign key_ ), maka nilai tersebut **harus sesuai dengan data yang ada di tabel tujuan** atau bernilai `NULL`.

Tujuan utama adalah:
1. **Mencegah data yang tidak valid atau tidak konsisten** .
2. **Menjaga hubungan antar-tabel tetap valid**.

![[Pasted image 20250607101209.png]]

## 3. Pemilihan Tipe Data yang Tepat
**FUN FACT:** Gue pernah buat system khusus audit. Gue pernah salah menetapkan tipe data yg harusnya "TEXT" malah "VARCHAR(128)" yg alhasil auditor sudah cape-cape ngetik malah kepotong. **INI FATAL!**

Contoh penggunaan berdasarkan case:
1. **`INT`**
    - **Case Cocok:** ID pengguna, jumlah barang, umur
    - **Alasan:** Cocok untuk angka biasa dalam rentang -2 miliar hingga +2 miliar
2. **`BIGINT`**
    - **Case Cocok:** Sistem global, hitungan besar (misal: klik, log)
    - **Alasan:** Untuk angka sangat besar yang melebihi kapasitas `INT`
3. **`SMALLINT` / `TINYINT`**
    - **Case Cocok:** Status aktif/nonaktif, bulan, hari
    - **Alasan:** Hemat space; cocok untuk nilai kecil dan terbatas
4. **`DECIMAL(p,s)`**
    - **Case Cocok:** Harga barang, gaji, nilai ujian
    - **Alasan:** Presisi tinggi; cocok untuk data finansial atau kuantitatif
5. **`FLOAT` / `DOUBLE`**
    - **Case Cocok:** Koordinat GPS, perhitungan ilmiah
    - **Alasan:** Cocok untuk angka desimal dengan toleransi pembulatan
6. **`CHAR(n)`**
    - **Case Cocok:** Kode negara (`ID`, `US`), kode pos tetap
    - **Alasan:** Panjang tetap; efisien untuk data pendek dan selalu sama panjang
7. **`VARCHAR(n)`**
    - **Case Cocok:** Nama pengguna, email, judul artikel
    - **Alasan:** Panjang variabel; hemat ruang jika isi berbeda-beda
8. **`TEXT` / `LONGTEXT`**
    - **Case Cocok:** Deskripsi produk, isi blog, log aktivitas/audit
    - **Alasan:** Menyimpan teks panjang tanpa batas tetap
9. **`DATE`**
    - **Case Cocok:** Tanggal lahir, tanggal order
    - **Alasan:** Menyimpan tanggal saja (tanpa waktu)
10. **`DATETIME` / `TIMESTAMP`**
    - **Case Cocok:** Waktu pendaftaran, log aktivitas
    - **Alasan:** Simpan tanggal & waktu; `TIMESTAMP` bisa otomatis update
11. **`BOOLEAN`**
    - **Case Cocok:** Status aktif/tidak, sukses/gagal
    - **Alasan:** Menyimpan nilai benar/salah (`1/0`, `true/false`)
12. **`ENUM`**
    - **Case Cocok:** Status pesanan (`pending`, `paid`, `shipped`)
    - **Alasan:** Batasi nilai input hanya pada opsi tertentu

## 4. Penggunaan Primary Key
**Primary Key (Kunci Utama)** adalah **kolom atau kombinasi beberapa kolom** dalam sebuah tabel yang digunakan untuk **mengidentifikasi setiap baris secara unik** .  
Artinya: tidak ada dua baris dalam tabel yang boleh memiliki nilai `PRIMARY KEY` yang sama.

Fungsi Utama Primary Key
- Memastikan keunikan data (Uniqueness).
- Menjadi dasar relasi antar-tabel (Relational Integrity).
- Biasanya otomatis terindeks, jadi pencarian lebih cepat.

![[Pasted image 20250607102800.png]]

![[Pasted image 20250607102837.png]]

## 5. Indexing pada Kolom yang Sering Diquery
**Indexing** adalah struktur data khusus yang dibuat untuk mempercepat pencarian dan akses data dalam tabel.  
Mirip seperti **indeks buku** : kamu tidak perlu membaca seluruh isi buku untuk menemukan halaman tertentu — cukup lihat indeksnya saja.

Analogi:
- Tabel = Buku
- Kolom = Bab atau topik
- Index = Indeks di belakang buku

**Mengapa Perlu Membuat Index?**
Saat jumlah data bertambah besar, query bisa menjadi **sangat lambat** jika tidak ada indeks. Dengan membuat **index pada kolom yang sering diquery** , kamu akan mendapatkan manfaat:

![[Pasted image 20250607103915.png]]

Baca: [[Jenis-Jenis Indexing]]

## 6. Beri Nama Kolom & Tabel Secara Konsisten

![[Pasted image 20250607104548.png]]

## Penutupan
Prinsip Dasar Desain Skema yang Baik
1. **Gunakan Normalisasi**
> Pastikan skema sudah dinormalisasi minimal sampai 3NF untuk menghindari redundansi dan anomali. 

2. **Pilih Primary Key dengan Bijak**
> Gunakan surrogate key (misalnya `INT AUTO_INCREMENT`) jika natural key bisa berubah atau sensitif.

3. **Gunakan Foreign Key untuk Relasi**
> Menjaga hubungan antar tabel tetap valid dan mencegah data "ngambang".

4. **Berikan Index pada Kolom yang Sering Diquery**
> Untuk meningkatkan performa pencarian dan join.

5. **Gunakan Tipe Data yang Sesuai**
> Jangan gunakan `VARCHAR(255)` untuk semua kolom. Pilih tipe data yang sesuai dengan jenis data.

6. **Beri Nama Kolom & Tabel Secara Konsisten**
> Contoh: `user_id`, bukan `userid` atau `id_user` di beberapa tempat.

**Improvement With Qwen AI**

Date: 07-06-2025