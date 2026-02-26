#Tech
# Table of Content
- [[#Introduction|Introduction]]
- [[#1. Tidak Memakai Relation pada Database|1. Tidak Memakai Relation pada Database]]
- [[#2. Cascade Delete Diaktifkan Tanpa Pertimbangan|2. Cascade Delete Diaktifkan Tanpa Pertimbangan]]
- [[#3. Menggunakan Query Builder Secara Sembarangan|3. Menggunakan Query Builder Secara Sembarangan]]
- [[#4. Tidak Ada Dokumentasi FSD (Functional Specification Document)|4. Tidak Ada Dokumentasi FSD (Functional Specification Document)]]
- [[#5. Lupa Menggunakan Server Side Processing|5. Lupa Menggunakan Server Side Processing]]
- [[#6. Tidak Menambahkan Indikator Loading|6. Tidak Menambahkan Indikator Loading]]
- [[#7. Adanya Redundansi di Database atau Kode|7. Adanya Redundansi di Database atau Kode]]

## Introduction
Sebagai seorang programmer, seringkali kita terburu-buru menyelesaikan masalah tanpa memikirkan konsekuensi jangka panjang. Padahal, kebiasaan buruk kecil di awal bisa menjadi bomerang nantinya.

Bomerang ketika aplikasi lo makin banyak usernya, makin kompleks. Berikut dosa yg gue lakuin sebagai programmer HAHA...

## 1. Tidak Memakai Relation pada Database
Seringkali karena ingin cepat atau merasa ngga perlu, relation antar tabel diabaikan. Tanpa relation, data bisa menjadi ngga konsisten, sulit dikelola, dan rawan duplikasi. Gue sering banget redundancy alhasil update ngga merata. 

## 2. Cascade Delete Diaktifkan Tanpa Pertimbangan

Cascade delete memang memudahkan proses penghapusan data secara otomatis, tapi jika tidak dipertimbangkan dengan matang, bisa menyebabkan hilangnya data yang seharusnya masih dibutuhkan.

Menurut gue kalo pake cascade delete yg gaboleh dihapus itu parent nya. bakal berdampak ke child (turunan nya semua). Takutnya dibutuhkan. Tapi kalo dari User oke aja, berarti gapapa.

## 3. Menggunakan Query Builder Secara Sembarangan
"Query builder itu enak, tapi bikin susah kalau aplikasi makin besar."

Walaupun query builder seperti Laravel Eloquent sangat membantu dalam penulisan dinamis, terlalu banyak logika kompleks di dalamnya membuat kode susah dibaca dan sulit dimaintain. Gue pernah ngelanjutin sistem dimana Full pake Query Builder. Alhasil gue pusing banget karena terlalu banyak join ditambah user minta nambah kolom.


Note: Best practice nya sih pake Eloquent ORM kalau di Laravel.
## 4. Tidak Ada Dokumentasi FSD (Functional Specification Document)
"Aplikasi tanpa FSD itu ibarat lo naik motor dengan mata tertutup."

FSD adalah panduan kerja yang memberikan gambaran jelas tentang apa yang harus dibuat dan bagaimana alur sistem bekerja. Tanpa dokumentasi ini, tim atau orang yg mau maintenance aplikasi lo bakal repot.

## 5. Lupa Menggunakan Server Side Processing
"Client side itu cepat, tapi nggak kuat kalau datanya banyak."

Menampilkan semua data di client side terlihat simpel dan penerapan secara teknis juga simpel, tapi saat jumlah data membesar, performa akan drop drastis. Server-side processing adalah solusi yang lebih stabil dan scalable.

Gue pernah nge test 5000 data dengan join ke 1 table. Lemot parah... Setelah diubah dari client ke server side jadi stabil. Jangan lupa Indexing.

## 6. Tidak Menambahkan Indikator Loading
"User mikir aplikasi error kalau nggak ada indikator loading."

Tanpa visual feedback seperti spinner atau loading text, user bisa bingung apakah aksi mereka sedang diproses atau tidak. Hal ini bisa menurunkan kepercayaan user terhadap aplikasi. Disatu sisi, gue ngalamin juga user itu klik 2x tombol submit saking ngga sabarannya. Alhasil data masuk double haha...

## 7. Adanya Redundansi di Database atau Kode
"Duplikasi itu tanda ketidakefisienan."

Redundansi baik di database maupun di codebase bisa menyebabkan inkonsistensi, kesulitan dalam maintenance, dan pemborosan sumber daya. Gue lebih sering ngalamin redundansi di sisi database. Itu mau normalisasi nya agak bingung karena sistem udah berjalan 24/7.

**Improvement With Qwen AI**

Date: 07-06-2025