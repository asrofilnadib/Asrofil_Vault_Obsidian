## 📋 Daftar Isi

1. [Overview Fundamental](#overview-fundamental)
2. [Konsep Dasar & Tujuan](#konsep-dasar--tujuan)
3. [Alur Bisnis (Business Flow)](#alur-bisnis-business-flow)
4. [Arsitektur Teknis](#arsitektur-teknis)
5. [Struktur Database](#struktur-database)
6. [Flow Proses Teknis](#flow-proses-teknis)
7. [Controller & Routing](#controller--routing)
8. [Tingkat SP & Kode SP](#tingkat-sp--kode-sp)
9. [Permission & Role](#permission--role)
10. [Email Notification](#email-notification)
---
## Overview Fundamental

  

### Apa itu SP Online?

  

**SP Online** adalah sistem digital untuk mengelola **Surat Peringatan (SP)** karyawan di PT. Prakarsa Alam Segar. Sistem ini menggantikan proses manual yang sebelumnya dilakukan dengan kertas, menjadi proses digital yang terintegrasi dan terotomasi.

  

### Definisi Surat Peringatan (SP)

  

Surat Peringatan adalah dokumen resmi yang diberikan perusahaan kepada karyawan sebagai bentuk peringatan tertulis atas pelanggaran atau ketidakdisiplinan yang dilakukan. SP memiliki tingkat-tingkat tertentu (SP1, SP2, SP3, SP3+) yang menunjukkan tingkat keseriusan pelanggaran.

  

### Mengapa SP Online Dibutuhkan?

  

1. **Efisiensi Proses**: Mengurangi waktu dan biaya dalam proses pembuatan SP

2. **Konsistensi Data**: Memastikan data SP tersimpan dengan konsisten dan terstruktur

3. **Tracking & Monitoring**: Memudahkan tracking history SP karyawan

4. **Audit Trail**: Setiap proses memiliki catatan lengkap (siapa, kapan, apa)

5. **Notifikasi Otomatis**: Email otomatis ke pihak terkait pada setiap tahap

6. **Reduksi Human Error**: Validasi otomatis mengurangi kesalahan manual

  

---

  

## Konsep Dasar & Tujuan

  

### Tujuan Sistem

  

1. **Digitalisasi Proses SP**: Mengubah proses manual menjadi digital

2. **Workflow Management**: Mengelola alur approval SP secara bertingkat

3. **Data Centralization**: Menyimpan semua data SP di satu tempat

4. **Compliance**: Memastikan proses SP sesuai dengan kebijakan perusahaan

5. **Reporting**: Memudahkan pembuatan laporan dan analisis SP

  

### Prinsip Dasar

  

1. **Multi-Level Approval**: SP harus melalui beberapa tahap approval sebelum aktif

2. **History Tracking**: Sistem mencatat semua history SP karyawan

3. **Automatic Calculation**: Sistem otomatis menghitung tingkat SP berdasarkan history

4. **Status Management**: Setiap SP memiliki status yang jelas (draft, approved, active, expired, cancelled)

5. **Role-Based Access**: Setiap user hanya bisa akses sesuai role dan permission-nya

  

### Jenis Pelanggaran yang Dikelola

  

1. **Alfa/Mangkir**: Karyawan tidak masuk kerja tanpa keterangan

2. **Pelanggaran**: Pelanggaran terhadap peraturan perusahaan (dapat diinput manual)

  

---

  

## Alur Bisnis (Business Flow)

  

### Flow Diagram Proses SP Online

  

```

┌─────────────────┐

│ Upload Data │ ← PIC upload data mangkir/pelanggaran

│ Mangkir/Alfa │

└────────┬────────┘

│

▼

┌─────────────────┐

│ Approve Data │ ← Approver approve data yang diupload

│ Alfa │

└────────┬────────┘

│

▼

┌─────────────────┐

│ SP Manager │ ← SP Manager konfirmasi & tentukan tingkat SP

│ Konfirmasi │

└────────┬────────┘

│

▼

┌─────────────────┐

│ Approve SP │ ← Dept Head IR approve SP final

│ (Dept Head IR) │

└────────┬────────┘

│

▼

┌─────────────────┐

│ SP Aktif │ ← SP aktif, bisa dicetak/dikirim ke karyawan

│ & Cetak │

└─────────────────┘

```

  

### Penjelasan Flow Bisnis

  

#### 1. Upload Data Mangkir/Alfa

- **Actor**: PIC (Person In Charge)

- **Aktivitas**: Upload file Excel berisi data karyawan yang mangkir/alfa

- **Output**: Data tersimpan dengan status `is_approved = N` (belum approve)

  

#### 2. Approve Data Alfa

- **Actor**: Approver (ditentukan per group)

- **Aktivitas**: Review dan approve data yang diupload

- **Output**: Data ter-approve (`is_approved = Y`), kasus SP dibuat

  

#### 3. SP Manager Konfirmasi

- **Actor**: SP Manager

- **Aktivitas**:

- Review kasus SP

- Sistem hitung potensi tingkat SP berdasarkan history

- SP Manager tentukan tingkat SP, kode SP, tanggal terbit, tanggal berakhir

- **Output**: Kasus dikonfirmasi (`is_confirmed = Y`), tingkat SP ditentukan

  

#### 4. Approve SP oleh Dept Head IR

- **Actor**: Dept Head IR (Industrial Relations)

- **Aktivitas**: Final approval SP

- **Output**: SP aktif (`is_depthead_ir_approved = Y`), email dikirim ke karyawan (kecuali SP3+)

  

#### 5. Cetak & Download SP

- **Actor**: SP Manager, Dept Head IR, atau PIC

- **Aktivitas**: Cetak/download SP dalam format PDF

- **Output**: File PDF SP siap dikirim ke karyawan

  

### Flow Tambahan

  

#### Revisi SP

- Karyawan atau pihak terkait bisa ajukan revisi SP

- Harus melalui approval process

  

#### Cancel SP

- SP bisa dibatalkan dengan alasan tertentu

- Harus melalui approval process

  

---

  

## Arsitektur Teknis

  

### Technology Stack

  

- **Framework**: Laravel (PHP)

- **Database**: MySQL

- **PDF Generation**: DomPDF

- **Email**: Laravel Mail (dengan queue job)

- **Frontend**: Blade Templates, jQuery, DataTables

  

### Struktur Folder

  

```

app/

├── Http/

│ └── Controllers/

│ └── SPOnline/

│ ├── UploadDataAlfaController.php

│ ├── ApproveDataAlfaController.php

│ ├── SPManagerController.php

│ ├── ApproveSPController.php

│ ├── PelanggaranController.php

│ ├── RevisiController.php

│ ├── CancelController.php

│ └── ...

├── Models/

│ └── SPOnline/

│ ├── Kasus.php

│ ├── DataAlfa.php

│ ├── Group.php

│ └── ...

└── Mail/

└── SPOnline/

├── SPOnlineMail.php

└── ...

  

resources/views/

└── sp-online/

├── upload-data-alfa.blade.php

├── approve-data-alfa.blade.php

├── sp-manager.blade.php

├── approve-sp.blade.php

└── ...

  

routes/

└── sp-online.php

  

database/migrations/

├── 2022_07_29_143319_create_sp_online_data_alfa_table.php

├── 2022_07_29_143333_create_sp_online_kasus_table.php

├── 2022_08_01_145921_create_sp_online_group_table.php

└── ...

```

  

---

  

## Struktur Database

  

### Tabel Utama

  

#### 1. `sp_online_data_alfa`

Tabel untuk menyimpan data alfa/mangkir yang diupload oleh PIC.

  

```sql

- id (bigint, primary key)

- nik (varchar 15) - Nomor Induk Karyawan

- nama (varchar 150) - Nama karyawan

- tanggal (date) - Tanggal mangkir

- nama_group (varchar) - Group/departemen

- approve_to (varchar) - NIK approver

- is_approved (enum: Y/N) - Status approval

- approved_by (varchar) - Username yang approve

- approved_at (datetime) - Waktu approval

- id_kasus (varchar 20) - ID kasus SP (nullable, diisi setelah approve)

- created_by (varchar 150) - Username yang upload

- created_at, updated_at (timestamps)

```

  

**Relasi**:

- `id_kasus` → `sp_online_kasus.id_kasus`

  

#### 2. `sp_online_kasus`

Tabel utama untuk menyimpan kasus SP.

  

```sql

- id (bigint, primary key)

- id_kasus (varchar 20, unique) - ID unik kasus

- jenis (enum: 'alfa', 'pelanggaran', 'manual') - Jenis kasus

- nik (varchar 15) - NIK karyawan

- nama (varchar 150) - Nama karyawan

- tanggal (date) - Tanggal pelanggaran

- tanggal_berakhir (date) - Tanggal berakhir SP (biasanya 3 bulan)

- tingkat_sp (enum: SP1, SP2, SP3, SP3+) - Tingkat SP

- kode_sp (varchar) - Kode SP (MANGKIR1, MANGKIR2+, dll)

- nomor_sp (int) - Nomor SP (auto increment per tahun, 0 untuk SP3+)

- nama_group (varchar) - Group/departemen

- bentuk_pelanggaran (text) - Deskripsi bentuk pelanggaran

- dasar_pertimbangan (text) - Dasar pertimbangan SP

  

-- Status Flags

- is_approved (enum: Y/N) - Status approval data alfa

- is_confirmed (enum: Y/N) - Status konfirmasi SP Manager

- is_depthead_ir_approved (enum: Y/N) - Status approval Dept Head IR

- is_aktif (enum: Y/N) - Status aktif kasus

- is_cancel (enum: Y/N) - Status cancel

- is_revised (enum: Y/N) - Status revisi

  

-- Approval Tracking

- created_by (varchar 150) - User yang create

- approved_by (varchar) - User yang approve data alfa

- approved_at (datetime) - Waktu approve data alfa

- confirmed_by (varchar) - SP Manager yang konfirmasi

- confirmed_at (datetime) - Waktu konfirmasi

- depthead_ir_approved_by (varchar) - Dept Head IR yang approve

- depthead_ir_approved_at (datetime) - Waktu approve Dept Head IR

- depthead_ir_approve_to (varchar) - NIK Dept Head IR yang harus approve

  

-- Revisi & Cancel

- cancel_progress (varchar) - Progress cancel

- cancel_is_approved (enum: Y/N) - Status approval cancel

- cancel_is_confirmed (enum: Y/N) - Status konfirmasi cancel

- is_revisi_confirmed (enum: Y/N) - Status konfirmasi revisi

- alasan_reject (text) - Alasan reject

  

-- Additional

- closing_sp3_plus (varchar) - Kode closing untuk SP3+

- created_at, updated_at (timestamps)

```

  

**Relasi**:

- `nama_group` → `sp_online_group.nama_group`

- `nik` → `karyawan.nik` (tabel karyawan)

  

#### 3. `sp_online_kasus_tanggal_pelanggaran`

Tabel untuk menyimpan detail tanggal pelanggaran (bisa multiple tanggal untuk satu kasus).

  

```sql

- id (bigint, primary key)

- id_kasus (varchar 20) - ID kasus

- tanggal (date) - Tanggal pelanggaran

- created_at, updated_at (timestamps)

```

  

**Relasi**:

- `id_kasus` → `sp_online_kasus.id_kasus`

  

#### 4. `sp_online_group`

Tabel untuk mengelola group/departemen dan mapping PIC, Approver, Dept Head IR.

  

```sql

- id (bigint, primary key)

- nama_group (varchar 150) - Nama group

- keterangan (varchar 250) - Keterangan group

- nik_pic (varchar 15) - NIK PIC (Person In Charge)

- nik_approver (varchar 15) - NIK Approver

- nik_depthead_ir (varchar 15) - NIK Dept Head IR

- is_active (enum: Y/N) - Status aktif

- created_at, updated_at (timestamps)

```

  

**Relasi**:

- `nik_pic`, `nik_approver`, `nik_depthead_ir` → `users.username` (tabel user)

  

#### 5. `sp_online_kode_sp`

Tabel master untuk mapping kode SP ke tingkat SP.

  

```sql

- id (bigint, primary key)

- kode (varchar) - Kode SP (MANGKIR1, MANGKIR2+, SP1+MANGKIR1, dll)

- tingkat_sp (enum: SP1, SP2, SP3, SP3+) - Tingkat SP

- bentuk_pelanggaran (text) - Deskripsi bentuk pelanggaran

- dasar_pertimbangan (text) - Dasar pertimbangan

- created_at, updated_at (timestamps)

```

  

**Contoh Data**:

- `MANGKIR1` → `SP1`

- `MANGKIR2+` → `SP1`

- `SP1+MANGKIR1` → `SP2`

- `SP2+MANGKIR1` → `SP3`

- `SP3+` → `SP3+`

  

### Tabel Pendukung

  

#### `sp_online_audit_mangkir`

Tabel untuk audit data mangkir.

  

#### `sp_online_karyawan_keluar`

Tabel untuk tracking karyawan yang keluar.

  

---

  

## Flow Proses Teknis

  

### 1. Upload Data Mangkir

  

**Controller**: `UploadDataAlfaController`

  

**Flow**:

1. PIC upload file Excel via `POST /sp-online/upload-data-mangkir/upload`

2. Sistem baca Excel, extract data (NIK, nama, tanggal, group)

3. Sistem cek duplikasi: `WHERE nik = ? AND tanggal = ? AND nama_group = ? AND is_approved = 'N'`

4. Jika tidak duplikat, insert ke `sp_online_data_alfa` dengan `is_approved = 'N'`

5. Sistem ambil `nik_approver` dari `sp_online_group` berdasarkan `nama_group`

6. Sistem kirim email ke approver

  

**Validasi**:

- File harus format Excel (.xls, .xlsx)

- Kolom wajib: NIK, Nama, Tanggal

- Cek duplikasi sebelum insert

  

### 2. Approve Data Alfa

  

**Controller**: `ApproveDataAlfaController`

  

**Flow**:

1. Approver buka halaman `GET /sp-online/approve-data-mangkir`

2. Sistem tampilkan data dengan `is_approved = 'N'` dan `approve_to = current_user.username`

3. Approver approve via `POST /sp-online/approve-data-mangkir`

4. Sistem update `sp_online_data_alfa`:

- `is_approved = 'Y'`

- `approved_by = current_user.username`

- `approved_at = now()`

5. Sistem generate `id_kasus` (format: `K{YYYYMMDD}{sequence}`)

6. Sistem insert ke `sp_online_kasus`:

- `id_kasus = generated_id`

- `jenis = 'alfa'`

- `nik, nama, tanggal` dari data alfa

- `is_approved = 'Y'`

- `is_confirmed = 'N'`

- `is_aktif = 'Y'`

7. Sistem insert tanggal ke `sp_online_kasus_tanggal_pelanggaran`

8. Sistem cek: jika mangkir 2 hari berturut-turut, buat `super_transaksi` dan `super_panggilan`

9. Sistem kirim email ke SP Manager

  

**Logic Khusus**:

- Jika karyawan mangkir 2 hari berturut-turut → otomatis buat panggilan ke-1

- Sistem hitung hari kerja (skip weekend/holiday)

  

### 3. SP Manager Konfirmasi

  

**Controller**: `SPManagerController`

  

**Flow**:

1. SP Manager buka halaman `GET /sp-online/sp-manager`

2. Sistem tampilkan kasus dengan:

- `is_approved = 'Y'`

- `is_confirmed = 'N'` atau `tingkat_sp IS NULL`

- `is_aktif = 'Y'`

- `jenis != 'manual'`

3. SP Manager klik kasus → `GET /sp-online/get-data-konfirmasi/{id_kasus}`

4. Sistem hitung potensi tingkat SP:

```php

// Ambil history SP aktif sebelumnya

$kasusAktif = Kasus::where('nik', $data->nik)

->where('tanggal_berakhir', '>', now())

->where('is_cancel', 'N')

->where('is_aktif', 'Y')

->where('is_confirmed', 'Y')

->get();

// Jika tidak ada history → SP pertama

if (count($kasusAktif) == 0) {

if ($data->jenis == 'alfa') {

$count = $tanggal_pelanggaran->count();

$kode_sp = $count > 1 ? 'MANGKIR2+' : 'MANGKIR1';

}

} else {

// Ada history → hitung berdasarkan tingkat SP sebelumnya

$prev = $kasusAktif->first();

if ($data->jenis == 'alfa') {

$kode = $prev->tingkat_sp . '+MANGKIR1';

// Cek jika berurutan → bisa jadi MANGKIR2+

}

}

// Ambil tingkat SP dari sp_online_kode_sp

$kodeSP = DB::table('sp_online_kode_sp')

->where('kode', $kode_sp)

->first();

$tingkat_sp = $kodeSP->tingkat_sp;

```

5. Sistem tampilkan potensi tingkat SP ke SP Manager

6. SP Manager pilih:

- Tingkat SP

- Kode SP

- Tanggal terbit SP

- Tanggal berakhir SP (default: 3 bulan dari tanggal terbit)

7. SP Manager konfirmasi via `POST /sp-online/konfirmasi-sp`

8. Sistem update `sp_online_kasus`:

- `is_confirmed = 'Y'`

- `confirmed_by = current_user.username`

- `confirmed_at = now()`

- `tingkat_sp = request->tingkat_sp`

- `kode_sp = request->kode_sp`

- `tanggal_berakhir = request->tanggal_berakhir`

- `nomor_sp = generate_nomor_sp()` (auto increment per tahun, 0 untuk SP3+)

- `depthead_ir_approve_to = group->nik_depthead_ir`

9. Sistem kirim email ke Dept Head IR

  

**Logic Generate Nomor SP**:

```php

if ($tingkat_sp == 'SP3+') {

$nomor_sp = 0;

} else {

$nomor_sp = generateID('', 'sp_online_kasus', 'nomor_sp',

DB::raw('YEAR(confirmed_at) = ' . date('Y')));

}

```

  

### 4. Approve SP oleh Dept Head IR

  

**Controller**: `ApproveSPController`

  

**Flow**:

1. Dept Head IR buka halaman `GET /sp-online/approve-sp`

2. Sistem tampilkan kasus dengan:

- `depthead_ir_approve_to = current_user.username`

- `is_confirmed = 'Y'`

- `is_depthead_ir_approved = 'N'`

- `is_aktif = 'Y'`

- `jenis != 'manual'`

3. Dept Head IR approve via `POST /sp-online/approve-sp`

4. Sistem update `sp_online_kasus`:

- `is_depthead_ir_approved = 'Y'`

- `depthead_ir_approved_by = current_user.username`

- `depthead_ir_approved_at = now()`

5. Jika `tingkat_sp != 'SP3+'`:

- Sistem kirim email ke karyawan via `SendSPOnlineMailJob` (queue job)

6. SP sekarang aktif dan bisa dicetak

  

**Reject Flow**:

1. Dept Head IR reject via `POST /sp-online/reject-sp`

2. Sistem update:

- `is_confirmed = 'N'`

- `confirmed_by = NULL`

- `confirmed_at = NULL`

- `tingkat_sp = NULL`

- `kode_sp = extract_kode_sp()` (untuk pelanggaran, ambil kode asli)

3. Kasus dikembalikan ke SP Manager untuk konfirmasi ulang

  

### 5. Cetak & Download SP

  

**Controller**: `SPManagerController` (method `download`)

  

**Flow**:

1. User buka halaman cetak SP

2. User pilih kasus → `GET /sp-online/download/{id_kasus}`

3. Sistem ambil data:

- Dari `sp_online_kasus`

- Dari `sp_online_kasus_tanggal_pelanggaran`

- Relasi ke `sp_online_group`, `users` (depthead, depthead_ir)

4. Sistem generate PDF menggunakan DomPDF

5. User download PDF

  

**Template PDF**:

- Nomor SP

- Tingkat SP

- Nama & NIK karyawan

- Tanggal pelanggaran (bisa multiple)

- Tanggal terbit & tanggal berakhir

- Tanda tangan Dept Head IR

  

---

  

## Controller & Routing

  

### Daftar Controller

  

1. **UploadDataAlfaController**

- `index()` - Halaman upload data mangkir

- `draftUpload()` - Preview data Excel sebelum upload

- `upload()` - Proses upload data

- `delete()` - Hapus data draft

  

2. **ApproveDataAlfaController**

- `index()` - Halaman approve data alfa

- `approve()` - Approve satu data

- `approveAll()` - Approve semua data

  

3. **SPManagerController**

- `index()` - Halaman SP Manager

- `data($status)` - Get data kasus berdasarkan status

- `getDataKonfirmasi($id)` - Get detail kasus untuk konfirmasi

- `konfirmasi()` - Proses konfirmasi SP

- `rejectKonfirmasi()` - Reject konfirmasi

- `print($id_kasus)` - Preview SP

- `download($id_kasus)` - Download PDF SP

- `closing()` - Closing SP3+

- `deleteTanggal()` - Hapus tanggal pelanggaran

  

4. **ApproveSPController**

- `index()` - Halaman approve SP

- `approve()` - Approve satu SP

- `approveAll()` - Approve semua SP

- `approveSelected()` - Approve selected SP

- `reject()` - Reject SP

- `rejectAll()` - Reject semua SP

- `rejectSelected()` - Reject selected SP

- `doApprove($id)` - Logic approve

- `doReject($id)` - Logic reject

- `sendEmail($kasus)` - Kirim email ke karyawan

  

5. **PelanggaranController**

- `index()` - Halaman upload pelanggaran

- `uploadFormPelanggaran()` - Upload form pelanggaran

- `approvePage()` - Halaman approve pelanggaran

- `approve()` - Approve pelanggaran

- `kirim()` - Kirim data pelanggaran

  

6. **RevisiController**

- `index()` - Halaman revisi SP

- `search($nik)` - Cari SP berdasarkan NIK

- `view($nik)` - Detail SP untuk revisi

- `kirim()` - Kirim pengajuan revisi

- `confirm()` - Konfirmasi revisi

  

7. **CancelController**

- `index()` - Halaman cancel SP

- `search($nik)` - Cari SP berdasarkan NIK

- `view($nik)` - Detail SP untuk cancel

- `kirim()` - Kirim pengajuan cancel

- `setujui()` - Setujui cancel

  

### Routing

  

File: `routes/sp-online.php`

  

**Route Groups**:

- Semua route menggunakan middleware: `auth`, `rules`, `access_log`

  

**Route Categories**:

  

1. **Upload Data Mangkir**

- `GET /sp-online/upload-data-mangkir`

- `POST /sp-online/upload-data-mangkir/draft-upload`

- `POST /sp-online/upload-data-mangkir/upload`

- `POST /sp-online/upload-data-mangkir/delete`

  

2. **Approve Data Alfa**

- `GET /sp-online/approve-data-mangkir`

- `POST /sp-online/approve-data-mangkir`

- `POST /sp-online/approve-data-mangkir-all`

  

3. **SP Manager**

- `GET /sp-online/sp-manager`

- `GET /sp-online/get-data-konfirmasi/{id}`

- `POST /sp-online/konfirmasi-sp`

- `POST /sp-online/reject-konfirmasi`

- `GET /sp-online/sp-manager/data/{status}`

- `GET /sp-online/print/{id_kasus}`

- `GET /sp-online/download/{id_kasus}`

- `POST /sp-online/closing`

- `POST /sp-online/delete-tanggal`

  

4. **Approve SP**

- `GET /sp-online/approve-sp`

- `POST /sp-online/approve-sp`

- `POST /sp-online/approve-sp-all`

- `POST /sp-online/approve-sp-selected`

- `POST /sp-online/reject-sp`

- `POST /sp-online/reject-sp-all`

- `POST /sp-online/reject-sp-selected`

  

5. **Pelanggaran**

- `GET /sp-online/pelanggaran`

- `POST /sp-online/upload-form-pelanggaran`

- `GET /sp-online/approve-pelanggaran`

- `POST /sp-online/approve-pelanggaran`

  

6. **Revisi**

- `GET /sp-online/revisi-sp`

- `GET /sp-online/revisi-sp/search/{nik}`

- `GET /sp-online/revisi-sp/{nik}`

- `POST /sp-online/revisi-sp/kirim`

- `POST /sp-online/revisi-sp/confirm`

- `GET /sp-online/approve-pengajuan-revisi`

- `POST /sp-online/approve-pengajuan-revisi`

- `GET /sp-online/approve-revisi`

- `POST /sp-online/approve-revisi`

  

7. **Cancel**

- `GET /sp-online/cancel-sp`

- `GET /sp-online/cancel-sp/search/{nik}`

- `GET /sp-online/cancel-sp/{nik}`

- `POST /sp-online/cancel-sp/kirim`

- `POST /sp-online/cancel-sp/setujui`

- `GET /sp-online/approve-pengajuan-cancel`

- `POST /sp-online/approve-pengajuan-cancel`

- `GET /sp-online/approve-cancel`

- `POST /sp-online/approve-cancel`

  

8. **Email & Follow Up**

- `POST /sp-online/send-email`

- `GET /sp-online/follow-up-email`

- `GET /sp-online/follow-up-email/data`

- `GET /sp-online/follow-up-email/logs/{id_kasus}`

- `GET /sp-online/follow-up-email/detail/{id}`

- `POST /sp-online/follow-up-email/send`

- `POST /sp-online/follow-up-email/update-email`

  

---

  

## Tingkat SP & Kode SP

  

### Tingkat SP

  

1. **SP1** - Surat Peringatan Pertama

2. **SP2** - Surat Peringatan Kedua

3. **SP3** - Surat Peringatan Ketiga

4. **SP3+** - Surat Peringatan Ketiga Plus (kondisi khusus)

  

### Kode SP

  

Kode SP adalah identifier untuk jenis pelanggaran. Mapping kode ke tingkat SP ada di tabel `sp_online_kode_sp`.

  

**Contoh Kode SP untuk Alfa/Mangkir**:

- `MANGKIR1` → SP1 (mangkir 1 hari, SP pertama)

- `MANGKIR2+` → SP1 (mangkir 2+ hari berturut-turut, SP pertama)

- `SP1+MANGKIR1` → SP2 (sudah ada SP1, mangkir lagi 1 hari)

- `SP1+MANGKIR2+` → SP2 (sudah ada SP1, mangkir 2+ hari berturut-turut)

- `SP2+MANGKIR1` → SP3 (sudah ada SP2, mangkir lagi 1 hari)

- `SP3+MANGKIR1` → SP3+ (sudah ada SP3, mangkir lagi)

  

**Logic Penentuan Kode SP**:

  

```php

// Jika SP pertama kali

if (tidak ada history SP aktif) {

if (jenis == 'alfa') {

if (jumlah_tanggal_pelanggaran > 1) {

kode_sp = 'MANGKIR2+';

} else {

kode_sp = 'MANGKIR1';

}

}

}

  

// Jika sudah ada history SP aktif

else {

$prev = history_SP_aktif[0]; // SP aktif terakhir

if (jenis == 'alfa') {

// Default: tingkat SP sebelumnya + MANGKIR1

kode_sp = $prev->tingkat_sp . '+MANGKIR1';

// Cek jika mangkir berurutan (hari kerja)

if (tanggal_pelanggaran == tanggal_SP_sebelumnya + 1 hari kerja) {

// Bisa jadi MANGKIR2+

kode_sp = $prev->tingkat_sp . '+MANGKIR2+';

}

}

// Jika sudah SP3+, tetap SP3+

if ($prev->tingkat_sp == 'SP3+') {

kode_sp = 'SP3+';

}

}

  

// Ambil tingkat SP dari tabel sp_online_kode_sp

$kodeSP = DB::table('sp_online_kode_sp')

->where('kode', $kode_sp)

->first();

$tingkat_sp = $kodeSP->tingkat_sp;

```

  

**Catatan Penting**:

- Sistem otomatis hitung tingkat SP berdasarkan history

- SP Manager bisa override jika diperlukan

- SP3+ tidak ada nomor SP (nomor_sp = 0)

- SP3+ tidak dikirim email ke karyawan

  

---

  

## Permission & Role

  

### Permission yang Digunakan

  

1. **`sp_online_upload_data_mangkir`**

- Akses: Upload data mangkir

- User: PIC

  

2. **`sp_online_approve_data_alfa`**

- Akses: Approve data alfa

- User: Approver (ditentukan per group)

  

3. **`sp_online_sp_manager`**

- Akses: SP Manager (konfirmasi SP, cetak SP)

- User: SP Manager

  

4. **`sp_online_approve_sp`**

- Akses: Approve SP final

- User: Dept Head IR

  

5. **`sp_online_upload_pelanggaran`**

- Akses: Upload pelanggaran

- User: PIC atau user tertentu

  

### Role Mapping

  

**PIC (Person In Charge)**:

- Username harus ada di `sp_online_group.nik_pic`

- Bisa upload data mangkir untuk group-nya

- Bisa cetak SP yang sudah aktif

  

**Approver**:

- Username harus ada di `sp_online_group.nik_approver`

- Bisa approve data alfa untuk group-nya

  

**SP Manager**:

- Harus punya permission `sp_online_sp_manager`

- Bisa akses semua group

- Bisa konfirmasi SP, cetak SP, manage SP

  

**Dept Head IR**:

- Username harus ada di `sp_online_group.nik_depthead_ir`

- Bisa approve SP final untuk group-nya

- Bisa cetak SP yang sudah aktif

  

---

  

## Email Notification

  

### Email yang Dikirim

  

1. **Email ke Approver** (setelah upload data mangkir)

- Subject: Notifikasi Data Mangkir Baru

- Content: Informasi data mangkir yang perlu di-approve

- Recipient: `sp_online_group.nik_approver`

  

2. **Email ke SP Manager** (setelah approve data alfa)

- Subject: Notifikasi Kasus SP Baru

- Content: Informasi kasus SP yang perlu dikonfirmasi

- Recipient: User dengan permission `sp_online_sp_manager`

  

3. **Email ke Dept Head IR** (setelah SP Manager konfirmasi)

- Subject: Notifikasi SP Perlu Approval

- Content: Informasi SP yang perlu di-approve

- Recipient: `sp_online_group.nik_depthead_ir`

- Mailer: `hrir` (khusus untuk HR IR)

  

4. **Email ke Karyawan** (setelah SP di-approve Dept Head IR)

- Subject: Surat Peringatan

- Content: Detail SP (tingkat SP, tanggal pelanggaran, dll)

- Recipient: Email karyawan (dari tabel `karyawan`)

- **Catatan**: Email ini TIDAK dikirim untuk SP3+

- Delivery: Via Queue Job (`SendSPOnlineMailJob`)

  

### Email Template

  

Email template ada di:

- `resources/views/mail/sp-online/`

  

### Queue Job

  

Email ke karyawan dikirim via queue job untuk performa:

- Job: `App\Jobs\SendSPOnlineMailJob`

- Queue: Default queue

- Mail: `App\Mail\SPOnlineMail`

  

---

  

## Status Kasus SP

  

### Status Flow

  

```

Draft (is_approved = N)

↓

Approved Data (is_approved = Y, is_confirmed = N)

↓

Confirmed (is_confirmed = Y, is_depthead_ir_approved = N)

↓

Active (is_depthead_ir_approved = Y)

↓

Expired (tanggal_berakhir < now())

```

  

### Status Flags

  

- **`is_approved`**: Data alfa sudah di-approve atau belum

- **`is_confirmed`**: SP Manager sudah konfirmasi atau belum

- **`is_depthead_ir_approved`**: Dept Head IR sudah approve atau belum

- **`is_aktif`**: Kasus aktif atau tidak (bisa di-reject jadi 'N')

- **`is_cancel`**: SP dibatalkan atau tidak

- **`is_revised`**: SP direvisi atau tidak

  

### Status di SP Manager

  

1. **need-confirmation**: Kasus yang perlu dikonfirmasi

- `is_approved = Y`

- `is_confirmed = N` atau `tingkat_sp IS NULL`

- `is_aktif = Y`

  

2. **approval**: Kasus yang menunggu approval Dept Head IR

- `is_confirmed = Y`

- `is_depthead_ir_approved = N`

- `is_aktif = Y`

  

3. **active_sp**: SP aktif (belum expired)

- `is_depthead_ir_approved = Y`

- `tanggal_berakhir >= now()`

- `is_cancel = N`

- `tingkat_sp != 'SP3+'`

  

4. **active_3_plus**: SP3+ aktif

- `is_depthead_ir_approved = Y`

- `tanggal_berakhir >= now()`

- `is_cancel = N`

- `tingkat_sp = 'SP3+'`

  

5. **not_active_sp**: SP sudah expired

- `is_depthead_ir_approved = Y`

- `tanggal_berakhir < now()`

  

6. **dibatalkan_sp**: SP yang dibatalkan

- `cancel_progress = 'disetujui'`

  

7. **pengajuan_pembatalan**: Pengajuan pembatalan SP

- `cancel_is_approved = Y`

- `cancel_is_confirmed = N`

  

8. **pengajuan_revisi**: Pengajuan revisi SP

- `is_revised = Y`

- `is_revisi_confirmed IS NULL`

  

9. **konfirmasi_ditolak**: Konfirmasi ditolak

- `is_aktif = 'N'`

- `alasan_reject IS NOT NULL`

  

---

  

## Fitur Tambahan

  

### 1. Revisi SP

- Karyawan atau pihak terkait bisa ajukan revisi SP

- Harus melalui approval process

- Controller: `RevisiController`

  

### 2. Cancel SP

- SP bisa dibatalkan dengan alasan tertentu

- Harus melalui approval process

- Controller: `CancelController`

  

### 3. Audit Mangkir

- Fitur untuk audit data mangkir

- Controller: `AuditMangkirController`

  

### 4. Follow Up Email

- Tracking email yang dikirim ke karyawan

- Bisa kirim follow up email

- Controller: `FollowUpEmailController`

  

### 5. Form Konseling

- Upload form konseling untuk karyawan

- Controller: `UploadFormKonselingController`

  

---

  

## Best Practices & Tips

  

### Untuk Developer

  

1. **Selalu cek status flags** sebelum update data

2. **Gunakan transaction** untuk operasi yang kompleks

3. **Validasi duplikasi** sebelum insert data

4. **Gunakan queue job** untuk email yang berat

5. **Log semua perubahan** penting untuk audit trail

  

### Untuk User

  

1. **Upload data tepat waktu** untuk memastikan proses cepat

2. **Cek data sebelum approve** untuk menghindari kesalahan

3. **Gunakan fitur preview** sebelum cetak SP

4. **Follow up email** jika karyawan belum terima email

  

---

  

## Troubleshooting

  

### Masalah Umum

  

1. **Email tidak terkirim**

- Cek queue worker jalan atau tidak

- Cek email karyawan valid atau tidak

- Cek mailer configuration

  

2. **SP tidak muncul di SP Manager**

- Cek status flags (`is_approved`, `is_confirmed`)

- Cek `is_aktif = 'Y'`

- Cek `jenis != 'manual'`

  

3. **Tingkat SP salah**

- Cek history SP aktif sebelumnya

- Cek mapping di `sp_online_kode_sp`

- Cek logic perhitungan di `SPManagerController`

  

4. **Nomor SP duplikat**

- Cek generate nomor SP per tahun

- Cek `nomor_sp` untuk SP3+ harus 0

  

---

  

## Referensi

  

- File dokumentasi sistem: `resources/views/pages/dokumentasi-sistem/index.blade.php`

- Route file: `routes/sp-online.php`

- Controller: `app/Http/Controllers/SPOnline/`

- Model: `app/Models/SPOnline/`

- Migration: `database/migrations/` (cari file `sp_online_*`)

  

---

  

**Dokumentasi ini dibuat untuk membantu developer memahami sistem SP Online secara fundamental dan teknis. Jika ada pertanyaan atau perlu update, silakan hubungi tim development.**


# Task
- [x] ➕ 2026-01-27  ketika sp2 sudah terbit dan besoknya admin input mangkir karyawan ybs menjadi sp3 📅 2026-01-28 ✅ 2026-01-27