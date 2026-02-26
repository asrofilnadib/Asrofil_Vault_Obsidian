# 🧾 QC Pending - Bapak Ramadhian

> Modul pengelolaan data pending pada Portal Quality Control, termasuk alur request, upload gambar, dan dashboard smart charting.

---

## 📌 Fitur & Alur Aplikasi
### 🧭 General Flow
- Setiap request dan post **dibuat melalui modal**. #Struggle 
- Pada halaman `Kelola Pending`: #Easy 
  - Tombol **Tambah** menggunakan modal.
  - Tombol **Edit Pending** juga menggunakan modal.

### 🖼️ Gambar / File Upload
- Semua image dapat di-**multi upload**. #Easy 
- Disimpan dalam kolom database dalam format **array JSON**. #Easy 
- **Catatan:**  
  Pada modal `Konfirmasi Tindak Lanjut Produksi`, **upload image tidak tersedia** ketika status pending sedang progress. #Easy  

### 🔐 Akses & Role
- Minta ke Pak Luki untuk mendapatkan:
  - **User Credential** role: `1`, `2`, `3`. #Easy 

### 📊 Smart Dashboard #Dashboard 
- Saat user menekan tombol **Pending Quality**, sistem langsung membuka tampilan **dashboard dengan chart pending** tanpa perlu input tambahan. #Easy 

---

## 🛠️ Meeting

### 📆 Meeting 14-07-2025 #AddFeatures  
- Bypass sistem login credential dari portal langsung menuju form page QC Pending.  
  `#Struggle`

---

## 📝 Notes Tambahan
- Disarankan menggunakan JSON decoding pada field gambar agar mudah diakses pada backend/frontend.
- Dashboard pending dapat menggunakan bar chart atau donut chart tergantung jumlah data yang tersedia.

---

## 🏷️ Tags
#QCPending #Struggle #Form #Modal #ImageUpload #Dashboard
