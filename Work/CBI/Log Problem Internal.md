# 🧾 Log Problem Internal - Bu Wiwin

> Modul pencatatan dan penanganan problem internal, terhubung dengan form PICA dan tampilan laporan.

---

## 🧱 Field Adjustment

### ✅ Tetap Digunakan (dengan modifikasi) #Easy 
- `Nama Tipe Unit` → gunakan **select2 (args)**
- `Nomor Tipe Unit` → gunakan **select2 (args)**
- `Problem Occurrence` → gunakan **select2 (args)**

### ❌ Dihilangkan
- `Merk Tipe Unit` → **dihapus**
- `Final Judgment` → **dihapus**
- `Action and Recommendation` → **dihapus**

### 🔁 Diganti
- `Issue Type` ➜ **diubah menjadi Lokasi**

---

## 🔄 Data Mapping Output
- **Resume Problem** dari log akan muncul pada tampilan **PICA Form**
  - Tujuannya untuk membantu tim memahami ringkasan masalah yang telah terjadi sebelumnya saat melakukan analisis penyebab dan tindakan.

---

## 📌 Summary
- Simplifikasi input data untuk unit dengan `select2` untuk kecepatan dan konsistensi.
- Fokus hanya pada data yang relevan dalam konteks internal.
- Penghapusan elemen yang tidak dibutuhkan untuk mempercepat pengisian form dan mengurangi kebingungan.
- Integrasi log dengan form PICA untuk menunjang investigasi dan rekomendasi tindak lanjut.

---
## 📅 #Meeting
### 🗓️ Meeting 14-07-2025 #AddFeatures
- bypass user login from portal and directly leads to form page #Struggle 
---
## 🏷️ Tags
#LogProblemInternal #Easy #Form #Select2 #PICA
