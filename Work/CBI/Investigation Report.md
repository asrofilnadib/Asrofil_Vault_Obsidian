# 📄 INVESTIGATION REPORT - Bapak Syafiq

### 🌐 [Portal Investigation Report](https://portal3.incoe.astra.co.id/qa-investigation-report/public/investigation "Investigation Report")

## 🧩 Bag A
- Input semua field dengan `select2` #Easy 
- Jika SN **baru** → tampilkan semua field lainnya (`d-block`) #Struggle 
- Jika SN **sudah ada** di database → fetch data otomatis & field lainnya disembunyikan (`d-none`) #Struggle 

---

## 🧩 Bag B
- Form dibuat dalam **bentuk tabel** #Easy 
- Terdapat tombol **Tambah** & **Hapus** #Easy 
- Setiap data fakta dapat disertai **gambar** (upload image) #Easy 

---

## 🧩 Bag C
- Terdiri dari 3 kolom: #Easy 
  - **Manufaktur** (pihak CBI)
  - **Distributor** (TN, SHN, BP, Nicirin, dsb.)
  - **Customer**
### 🏭 Manufaktur
- Pre Delivery Check:
  - Charger: OK / NOT OK
  - Discharge: OK / NOT OK

### 🚚 Distributor
- BAST:
  - Charger: OK / NOT OK
  - Discharge: OK / NOT OK

### 👥 Customer
- Status: **Diterima** / **Ditolak**
- Terdapat tombol **Add** & **Delete**

---
## 📅 #Meeting

### 🗓️ Meeting 23-06-2025 #AddFeatures 
#### 🧩 Dimuat dalam Bag D
![[IMG/investigation.jpg]]

| Komponen  | Jenis Unit | Brand Unit | Tipe Unit | Tipe Komponen | SN Komponen | Kesimpulan 1               | Kesimpulan 2 | Kesimpulan 3           | Kesimpulan 4 |
| --------- | ---------- | ---------- | --------- | ------------- | ----------- | -------------------------- | ------------ | ---------------------- | ------------ |
| 🔋Battery |            |            |           | Tipe Battery  | SN Battery  | Problem Knowledge          | Design Flaw  | Improper Manufacturing | Others       |
| 🔌Charger |            |            |           | Tipe Charger  | SN Charger  | Mistake Electrical Install | Misoeration  | Design Flaw            | Others       |

#### ✍️ Tanda Tangan #Struggle 
- Ganti label “Signatures” → **Tanda Tangan**
- Saat form disubmit:
  - Muncul tombol **Preview Doc** di kolom Action
  - Preview memuat seluruh data yang di-submit
  - User dapat **paraf** & isi **nama**
  - Setelah submit → baru muncul tombol **Export PDF** yang sudah memuat paraf

### 🗓️ Meeting 26-06-2025 #AddFeatures 
Pada tombol `detail-investigation`:
- Tambahkan tombol **Hapus Tanda Tangan** #Struggle 
- Tambahkan field untuk **upload file (image only)** di bagian tanda tangan #Struggle 
![[IMG/Pasted image 20250718200137.png]]
- Field remark:
  - Bisa dalam bentuk gambar saja atau nama peserta saja atau keduanya #Easy 
![[IMG/Pasted image 20250718200239.png]]

### 🗓️ Meeting 03-07-2025 #Revision #AddFeatures 
pada modal form investigation:
- Tambah User; Plant; Station #Easy 
- Tiap tindakan perbaikan _(corrective action)_ berikan keterangan statusnya `closed` & `open` #Struggle 
- Kesimpulan fixed dengan yang di gambar  ![[IMG/investigation report  1.jpg]] 

## 📝 Documentation App of Investigation Report

### Detail Investigation
![[IMG/screencapture-localhost-8080-investigation-2025-07-18-20_02_50.png]]
### Form Investigation
![[IMG/screencapture-localhost-8080-investigation-2025-07-18-20_06_48.png]]
### Export File Investigation
![[Investigation-Report_21_2025-07-18.pdf]]