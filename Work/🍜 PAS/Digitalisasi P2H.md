# 🚛 Digitalisasi P2H (Pre-Operational Check) - WFG
<iframe
  style="border:1px solid rgba(0, 0, 0, 0.1);"
  width="100%"
  height="450"
  src="https://www.figma.com/embed?embed_host=obsidian&url=https://www.figma.com/design/LpPt4ww48ZM4yy27D8VMTx/E-P2H?node-id=0-1&p=f"
  allowfullscreen>
</iframe>
---

> *"Menggantikan form manual dengan sistem digital untuk monitoring kerusakan MHE secara real-time, otomatisasi ticket downtime, dan analitik produktivitas."*

- **Project Owner**: Dani Wiraseno
- **PIC Teknis**: Mohammad Asrofil Nadib
- **Sistem Terkait**: PAS Internal / SMU Tab App (TBD)
- **Status**: Planning
- **Created**: 30 June 2025
- **Prioritas**: 🔴 High
- **Lokasi**: Area WFG
- **Figma Mockup**: [E-P2H Figma](https://www.figma.com/design/LpPt4ww48ZM4yy27D8VMTx/E-P2H?m=auto&t=yiR05SatY2EANVbx-6)

---

## 🎯 Tujuan Proyek

Menggantikan **form P2H manual berbasis kertas** dengan sistem digital yang memungkinkan:
- Pengisian checklist oleh operator via aplikasi
- Otomatisasi pembuatan tiket downtime saat ditemukan kerusakan
- Monitoring real-time oleh foreman
- Analisis historikal untuk:
  - Produktivitas MHE
  - Trend kerusakan
  - Evaluasi kontribusi human error
  - Persiapan spare part & preventive maintenance

---

## ⚠️ Permasalahan Saat Ini (Before)

| Masalah                             | Dampak                                        |
| ----------------------------------- | --------------------------------------------- |
| Form P2H manual (kertas)            | Sulit tracking, rawan hilang, tidak real-time |
| Input manual untuk analisis         | Waktu lama, rentan error                      |
| Tidak bisa tambah perlengkapan baru | Flexibilitas rendah                           |
| Tidak ada notifikasi otomatis       | Delay perbaikan                               |
| Tidak ada data historikal terpusat  | Analisa kinerja & prediksi sulit              |

---

## ✅ Solusi Digital (After)

| Fitur | Deskripsi |
|------|-----------|
| 🖥️ Aplikasi Digital P2H | Diisi oleh operator sebelum operasional (via tablet/HP) |
| 🔄 Sync Data Otomatis | Integrasi dengan backend PAS atau aplikasi tab SMU |
| 🧩 Master Data | Kelola MHE, Checklist, Operator, PIC |
| 📝 Form Checklist Interaktif | Jika temuan "Breakdown", muncul popup detail kerusakan |
| 🎯 Auto Create Ticket | Saat submit dengan jenis "Breakdown" → otomatis buat tiket di Portal Downtime |
| 🔔 Notifikasi Otomatis | Kirim alert ke tim ENG (Engineering) via email/Telegram |
| 📊 Dashboard Foreman | Lihat semua status P2H, trend kerusakan, produktivitas |
| 📈 Analitik Lanjutan | Data siap dikembangkan untuk predictive maintenance |

---

## 🔧 MVP Scope (Minimum Viable Product)

Fitur wajib dalam tahap awal:

```text
- Master MHE (User Pak Dani)
- Master Checklist MHE (User Pak Dani)
- Master Operator & PIC (User Pak Dani)
- Form Checklist (Operator)
- Confirm & Monitoring (Foreman)
- Auto Create Ticket Downtime (on submit type breakdown)
- Data Produktivitas MHE by Unit vs Breakdown Time (Foreman)
- Data trend kerusakan MHE untuk persiapan Spare Part dan Preventive action (Foreman)
- Data Operator yang MHE nya sering rusak untuk mengetahui apakah ada kontribusi Human Error (cara pakai yang salah) (Foreman)
```

---

## 👥 User Role & Akses

| Role | Hak Akses |
|------|----------|
| **Operator** | Isi form P2H, upload foto (jika ada kerusakan) |
| **Foreman** | Monitor semua form, konfirmasi, lihat dashboard analitik |
| **PIC (Pak Dani)** | Kelola master data, ekspor laporan, setting notifikasi |
| **ENG Team** | Terima notifikasi, lihat tiket di Portal Downtime |
| **Admin PAS** | Maintenance sistem, backup data, troubleshooting |

---

## 📐 Arsitektur Sistem (High-Level)

```
[Operator Tablet/App]
        ↓ (input data)
[PAS Backend / SMU Tab Integration]
        ↓
[Database: MHE, Checklist, Tickets]
        ↓
[Auto Trigger: Create Downtime Ticket]
        ↓
[Notification: Telegram/Email → ENG]
        ↓
[Dashboard: Foreman & PIC]
```

> 🔹 Opsi integrasi:
> - Embed di aplikasi tab operator (SMU)
> - Aplikasi terpisah (internal PAS) — lebih fleksibel

---

## 🗺️ Milestone Proyek

### 📅 Fase 1: Planning & Design (Week 1–2)
- [ ] Finalisasi scope MVP
- [ ] Review Figma mockup bersama user
- [ ] Tentukan pilihan integrasi: embed atau standalone?
- [ ] Desain database (Master MHE, Checklist, Operator, dll)
- [ ] Buat ERD sederhana
- [ ] Konfirmasi alur notifikasi (Telegram/email?)

### 📅 Fase 2: Development (Week 3–6)
- [ ] Setup backend Laravel (Ubuntu 24.04)
- [ ] Implementasi master data (CRUD)
- [ ] Buat form checklist dinamis (blade + JS)
- [ ] Validasi: jika `breakdown` → wajib isi detail
- [ ] Integrasi FilePond untuk upload foto kerusakan
- [ ] Auto-create ticket ke Portal Downtime (API call)
- [ ] Kirim notifikasi Telegram saat ticket dibuat

### 📅 Fase 3: Testing & UAT (Week 7)
- [ ] Uji coba internal (team PAS)
- [ ] Simulasi pengisian oleh operator (test user)
- [ ] Cek notifikasi masuk ke ENG
- [ ] Validasi data muncul di dashboard foreman
- [ ] Fix bug minor

### 📅 Fase 4: Deployment & Training (Week 8)
- [ ] Deploy ke server produksi
- [ ] Training singkat untuk operator & foreman
- [ ] Dokumentasi user guide (PDF/obsidian)
- [ ] Go-live di area WFG (pilot)

### 📅 Fase 5: Monitoring & Iterasi (Ongoing)
- [ ] Pantau usage selama 2 minggu pertama
- [ ] Kumpulkan feedback dari user
- [ ] Rencanakan fitur lanjutan:
  - Predictive maintenance
  - Export to Excel/CSV
  - Mobile app version
  - Approval flow

---

## 📊 Benefit & KPI

| Benefit | Target |
|--------|--------|
| ✅ Eliminasi input manual | 100% digital |
| ⏱️ Reduksi waktu analisa | Dari 1 hari → real-time |
| 📉 Turunkan downtime | 15% dalam 3 bulan |
| 🔧 Optimalisasi spare part | Prediksi kebutuhan bulanan |
| 👤 Identifikasi human error | ≥3 kasus/tahun terdeteksi |
| 📢 Notifikasi cepat ke ENG | <5 menit setelah submit |

---

## 🔗 Referensi & Attachment
- [Figma Mockup - E-P2H](https://www.figma.com/design/LpPt4ww48ZM4yy27D8VMTx/E-P2H?m=auto&t=yiR05SatY2EANVbx-6)
- [[Digitalisasi P2H - Flowchart.drawio]]
- [[Portal Downtime Integration Spec]]

---

## 📝 Catatan Tambahan
- Pastikan form responsif untuk mobile/tablet
- Gunakan `selectize.js` untuk input multi-tag (keterangan kerusakan)
- Backup harian ke cloud (Google Drive via script)
- Logging aktivitas penting (create ticket, edit master)
- Backup plan: tetap sediakan form cadangan (print) selama masa transisi

> 💬 *"Tujuan akhir: sistem ini menjadi fondasi predictive maintenance untuk seluruh aset MHE di WFG."*

---

# 📋TASK
- [x] ➕ 2025-12-31 buat ui untuk master data P2H 🔽 📅 2026-01-01 ✅ 2025-12-31
- [x] ➕ 2025-12-31 buat ui untuk opreator scan sampai tahap submit inspeksi 🔽 📅 2025-12-01 ✅ 2025-12-31
- [x] ➕ 2026-12-31 bakal dikasih master datanya pada tanggal ⏳ 2026-01-05 ✅ 2026-01-05
- [x] ➕ 2026-01-31 buat ui untuk list table of inspeksi, sekalian confirm tanda tangan foreman 🔽 📅 2026-01-02 ✅ 2026-01-02
- [x] ➕ 2026-01-06 membuat relasi database dengan master datanya ⏫ 📅 2026-01-08 ✅ 2026-01-06
- [x] ➕ 2026-01-06 integrasi CRUD di master data MHE 🔼 📅 2026-01-07 ✅ 2026-01-06
- [x] ➕ 2026-01-06 integrasi CRUD di master data Inspeksi, list inspeksi untuk mhe tertentu 🔼 📅 2026-01-07 ✅ 2026-01-07
- [x] ➕ 2026-01-06 integrasi CRUD di master data mhe types 🔼 📅 2026-01-07 ✅ 2026-01-07
- [x] ➕ 2026-01-06 integrasi CRUD di master data users 🔼 📅 2026-01-07 ✅ 2026-01-07
- [x] ➕ 2026-01-07 integrasi fetch data inspeksi sesuai dengan tipe mhe nya 🔼 📅 2026-01-09 ✅ 2026-01-07
- [x] ➕ 2026-01-07 diatas form checklist ada inputan data mhe. Departemen, tipe, no MHE itu ambil dari scannya; hours shift input manual 🔽 📅 2026-01-08 ✅ 2026-01-08
- [x] ➕ 2026-01-07 buat validasi: ✅ 2026-01-08
	- format checklist
	- tipe mhe yang sesuai
	- item checklist tidak sesuai dengan tipe mhe nya
	- isi semua checklist
	- insiasi form pada shift tersebut sudah dilakukan
	- detail kerusakan: wajib diisi
- [x] ➕ 2026-01-08 implementasi alur logika: scan yang isinya adalah nomor tipe mhe nya lalu masukan ke form, setelah form diisi dan disimpan kembali lagi ke scan-aktif 🔼 📅 2026-01-09 ✅ 2026-01-08
- [x] ➕ 2026-01-08 filtering inspeksi data di /p2h/inspeksi 🔽 📅 2026-01-09 ✅ 2026-01-08
- [x] ➕ 2026-01-09 buat agar user yang ketika dia belum scan mhe, maka tidak bisa akses formnya, jadi alurnya scan -> form -> redirect scan 🔼 📅 2026-01-13 ✅ 2026-01-13
- [x] ➕ 2026-01-13 create export button pdf sekaligus layouting pdf nya 🔼 📅 2026-01-14 ✅ 2026-01-13
- [x] ➕ 2026-01-14 hasil output dari pdf harusnya sama untuk no dokumen yang sama, jadi output pdf "FB15202601132" "FB15202601133" harusnya sama 🔼 📅 2026-01-15 ✅ 2026-01-14
- [x] ➕ 2026-01-16 konfigurasi migrasi master data dari p2h ke downtime ⏫  📅 2026-01-17 ✅ 2026-01-16
- [x] ➕ 2026-01-14 ketika user sedang inspeksi lalu menemukan kerusakan part, ketika form tersebut di create langsung membuat tiket di downtime ⏫ 📅 2026-01-16 ✅ 2026-01-15
- [x] ➕ 2026-01-15 membuat dashboard untuk p2h 🔼 📅 2026-01-17 ✅ 2026-01-16
- [x] ➕ 2026-01-18 Melakukan perancangan integrasi master data P2H dengan sistem Downtime agar setiap perubahan data oleh SPV otomatis tersinkron ke kedua sistem 🔼 📅 2026-01-20 ✅ 2026-01-20
- [x] ➕ 2026-01-18 Membuat BOT TelegramService untuk memberikan notifikasi kepada teknisi terkait yang di assign oleh Bapak Dani Wiraseno selaku Chief ⏫ 📅 2026-01-21 ✅ 2026-01-20
- [x] ➕ 2026-07-29📅 untuk impact ganti jadi: low; medium; high. Apapun impactnya bakal create ticket 🔼 ✅ 2026-07-30
  ![[Pasted image 20260729161729.png]]
- [x] ➕ 2026-07-29 setelah create checklist maka tampilan dari operator adalah checklist yang di create, detail dengan on progress ticket downtime. ✅ 2026-07-30
- hourse meter di ilangin.
- Case dimana kalau temuan kerusakan setelah shift jalan, maka operator akses checklist sebelumnya lalu tambah kerusakan
- secara tampilan form, beri badge atau alert kalau ini
  - di detail kerusakan tambahin detail kerusakan jadi 4 kategori: mechanical, hidrolik, other, electrical. Kalau other baru detail kerusakan free text
  - ![[Pasted image 20260729161917.png]]
  - ![[Pasted image 20260729163349.png]]
- [x] ➕ 2026-07-29 case kalau shift 1 unit FB-15 kerusakan spion kemudian sampai shift 2 belum dibenerin maka si operator checklist pada unit FB-15 yang sama dengan pengecekan spion dibuat seperti disable gitu dengan label masih rusak atau sedang on progress repairement ✅ 2026-07-30
- [x] ➕ 2026-07-29 buat halaman display monitoring untuk melihat kerusakan p2h, user ingin tampilannya itu bisa di breakdown berdasarkan hari; impact; unit mhe, jadi gw mikirnya itu dibuat kayak collapse yang turunan turunan terus. ✅ 2026-07-30
- [x] ➕ 2026-07-29 untuk master data di downtime, mesin dijadikan concat mhe tipe + mhe unit, equipment itu jadinya checklist, dan kerusakan itu cuma ada 4 kategori ✅ 2026-07-30
  - ![[Pasted image 20260729162847.png]]
- ![[Pasted image 20260729162940.png]]

# 🕙 Pending
- [x] minta master data ke pak dani ✅ 2026-01-05
- [x] buat master data pengecekan untuk masing masing unit ✅ 2026-01-07
- [ ] add master data di downtime
      - workcenter: Warehouse
      - Area: WFG
      - Mesin: Reach Truck; Forklift; Loader; Truck; PM
      - equipment: sesuaikan dengan yang ada di P2H (follow up juga ke pak dani)
	- [ ] minta penjelasan lebih detail mengenai bagian summary, apakah itu pershift?
