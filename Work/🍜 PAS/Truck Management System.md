#pas 

# Flowchart
![[IMG/DOCUMENT FLOWCHART TMS.drawio.png]]
# Alur secara naratif
- Produksi upload [**data reservasi material dari SAP**](obsidian://open?vault=Asrofil%20Vault&file=Work%2FPAS%2FTMS%20Drawing.excalidraw) → masuk ke sistem.
	
- WSM upload **picking list** berdasarkan data reservasi.
    
- Staging memastikan material siap diangkut.
    
- Pengawas membuat draft transaksi & _request order_ ke driver.
    
- Driver menerima, jalan ke lokasi, memuat barang, lalu kirim ke tujuan.
    
- Setelah bongkar, Produksi melakukan konfirmasi kedatangan.
    
- Jika ada masalah, dilakukan _complain handling_ oleh WSM.
    
- Semua proses tercatat otomatis dan dapat dimonitor via dashboard & peta digital.

| Role                | Tanggung Jawab                                                     | Entity Table Terkait                               |
| ------------------- | ------------------------------------------------------------------ | -------------------------------------------------- |
| **Admin Produksi**  | Upload data reservasi dari SAP (outgoing/incoming material).       | `tms_material_reservasi`                           |
| **Admin WSM**       | Upload Picking List dan input data NTI (Nomor Transaksi Internal). | `tms_material_reservasi_wsm`                       |
| **Staging**         | Melakukan checklist dan validasi fisik material sebelum diangkut.  | `tms_transaction_material`                         |
| **Pengawas Driver** | Membuat draft transaksi dan melakukan _request order_ ke driver.   | `tms_transaction`, `tms_transaction_request_order` |
| **Driver**          | Melaksanakan seluruh proses pengambilan dan pengiriman barang.     | `tms_driver`, `tms_transaction`, `tms_car_history` |

# Alur Proses Bisnis TMS

### 🟩 1. Upload Reservasi (Produksi)
**Aktivitas:**
Admin Produksi mengunggah file Excel hasil export dari SAP berisi data material yang akan dikirim.

**Mapping Data (Excel → Database):**

| Excel Field  | DB Field             |
| ------------ | -------------------- |
| No Reservasi | `no_reservasi`       |
| Nama Barang  | `nama_barang`        |
| Batch        | `batch`              |
| Jumlah       | `jumlah_som`         |
| Satuan       | `uom`                |
| Shift        | `shift`              |
| Tujuan       | `asal` / `recipient` |

**Tabel:** `tms_material_reservasi`

---

### 🟨 2. Upload Picking List (Admin WSM)
**Aktivitas:**
Admin WSM mengunggah Picking List dari SAP (setelah upload Produksi selesai).

**Mapping Data:**

| Field         | Keterangan              |
| ------------- | ----------------------- |
| `no_doc`      | Nomor dokumen SAP       |
| `to_dummy`    | Tempat tujuan sementara |
| `mid`         | Material ID             |
| `nama_barang` | Nama material           |
| `qty_to`      | Jumlah yang dikirim     |
| `status`      | Status proses picking   |

**Tabel:** `tms_material_reservasi_wsm`

---

### 🟦 3. Checklist Material (Staging)
**Aktivitas:**
Tim Staging memverifikasi fisik material berdasarkan Picking List.

**Tabel:** `tms_transaction_material`

**Mapping:**
| Field | Keterangan |
|--------|-------------|
| `nama_barang` | Material yang dicek |
| `total_palet`, `total_som` | Jumlah konversi hasil pengecekan |
| `tms_master_konversi_palet` | Referensi satuan konversi |

---

### 🟥 4. Create Draft (Pengawas)
**Aktivitas:**
Pengawas membuat *draft transaksi* untuk menentukan jenis pengiriman:
- `Incoming` → Barang masuk ke gudang  
- `Outgoing` → Barang keluar dari gudang

**Tabel:** `tms_transaction`

**Field Penting:**

| Field | Keterangan |
|--------|-------------|
| `jenis_transaksi` | Incoming / Outgoing |
| `status` | Draft |
| `from`, `to` | Asal dan tujuan |
| `schedule_tanggal` | Jadwal pengiriman |

---

### 🟧 5. Request Order (Pengawas)
**Aktivitas:**
Pengawas mengubah draft menjadi *request order* agar muncul di dashboard driver.

**Efek Sistem:**
- Trigger notifikasi via WebSocket ke akun driver
- Jika driver menolak → sistem auto-assign ke driver lain

**Tabel:** `tms_transaction_request_order`

---

### 🟩 6. Terima Order (Driver)
**Aktivitas:**
Driver menerima order dari aplikasi.
- Jika **accepted** → lanjut ke pengambilan
- Jika **rejected** → order dipindahkan

**Status:** `accepted` / `rejected`

---

### 🟦 7. Input NTI (Admin WSM)
**Aktivitas:**
Admin WSM mengisi *Nomor Transaksi Internal (NTI)* untuk menandai dokumen pengiriman.

**Tabel:** `tms_doc_control`

| Field | Keterangan |
|--------|-------------|
| `doc_no` | Nomor dokumen |
| `jenis_document` | Jenis transaksi |
| `tanggal` | Tanggal dibuat |
| `cost_center` | Kode departemen |
| `status` | Status approval |

---

### 🟨 8. Mulai Jalan Pengambilan (Driver)
**Aktivitas:**
Driver menekan tombol “Mulai Jalan”.  
**Status:** `on_the_way_pickup`

**Tabel:** `tms_transaction`
- Catat waktu (`schedule_berangkat`)
- Simpan posisi GPS (via `lokasi_driver`)

---

### 🟩 9. Tiba Pengambilan (Driver)
**Aktivitas:**
Driver menekan tombol “Tiba” ketika sampai di lokasi pengambilan barang.

**Update:** `status` → `arrived_pickup`

---

### 🟧 10. Muat Barang (Driver)
**Aktivitas:**
Driver melakukan *scan barcode material* saat proses muat barang.

**Validasi:**
- Pastikan data konversi tersedia di `tms_master_konversi_palet`

**Tabel:** `tms_transaction_material`

---

### 🟦 11. Mulai Jalan Pengiriman (Driver)
**Aktivitas:**
Setelah muat selesai, driver mulai pengiriman ke lokasi tujuan.

**Update:**
- `status` → `on_the_way_delivery`
- Lokasi realtime tersimpan di `tms_car_history`

---

### 🟨 12. Tiba Pengiriman (Driver)
**Aktivitas:**
Driver tiba di lokasi tujuan dan *scan QR Code gedung* pengawas.

**Tabel:** `tms_master_gedung`

---

### 🟥 13. Bongkar Barang (Driver)
**Aktivitas:**
Driver melakukan:
- Scan “Mulai Bongkar”
- Scan “Selesai Bongkar”

**Status:**
`unloading_start` → `unloading_complete`

**Tabel:** `tms_transaction`

---

### 🟦 14. History Pengiriman (Driver)
**Aktivitas:**
Seluruh aktivitas driver dicatat otomatis.

**Tabel:** `tms_car_history`

| Field             | Keterangan             |
| ----------------- | ---------------------- |
| `status_truck`    | Aktif / Breakdown      |
| `lokasi_terdekat` | Lokasi terakhir        |
| `foto_breakdown`  | Bukti jika ada masalah |
| `keterangan`      | Catatan driver         |

---

### 🟩 15. Konfirmasi Barang (Produksi)
**Aktivitas:**
Admin Produksi melakukan verifikasi kedatangan barang.

**Status:** `confirmed`
**Tabel:** `tms_material_reservasi`

---

### 🟧 16. Complain Barang (Produksi) & Complain Data (Admin WSM)
**Aktivitas:**
Jika ada ketidaksesuaian (jumlah/rusak):
- Produksi membuat complain barang
- WSM menindaklanjuti via data complain

**Tabel:** `tms_material_reservasi_wsm`

| Field | Keterangan |
|--------|-------------|
| `nik_complain` | Pembuat komplain |
| `is_complain` | Flag komplain aktif |
| `wsm_response` | Tindakan dari WSM |

---

### 🟦 17. Monitoring & Dashboard
**Aktivitas:**
Semua peran dapat melihat status pengiriman melalui dashboard & peta digital.

**Dashboard:**
- Incoming
- Outgoing
- All Summary

**Peta Digital:**
- Tracking realtime dari `tms_car_history`

---

## 🔗 Relasi Utama
| Proses | Tabel | Relasi ke | Keterangan |
|--------|--------|------------|-------------|
| Upload Reservasi | `tms_material_reservasi` | `tms_material_reservasi_wsm` | Data SAP awal |
| Draft & Request | `tms_transaction` | `tms_transaction_request_order` | Membuat draft transaksi |
| Eksekusi | `tms_driver` | `tms_transaction`, `tms_car` | Menjalankan order |
| Material | `tms_transaction_material` | `tms_master_konversi_palet` | Konversi unit |
| Dokumen | `tms_doc_control` | `tms_transaction` | Menyimpan bukti transaksi |
| Tracking | `tms_car_history` | `tms_driver`, `tms_car` | Monitoring posisi dan histori |

---

> 📌 **Catatan:**
> - Semua notifikasi dan update status menggunakan WebSocket.
> - Error input jumlah stok biasanya karena konversi palet belum diset oleh Admin WSM.
> - Sistem saling terhubung antara Produksi, WSM, Staging, Pengawas, dan Driver.

# Class Diagram
![[IMG/tms 1.png]]


# improvement:
> ➕ 2025-10-29 tiap improvement atau perubahan yang ada di project tms, disesuaikan dengan yang ada di excel dataKey_end 🔁 
## Tasks
- [x] ➕ 2025-10-29 pengawas request, tambahkan badge untuk tiap header navigasi: draft, request bak, waiting, dsb. 🔽 📅 2025-10-31 ✅ 2025-10-30
- [x] ➕ 2025-10-29 order by desc untuk daftar order, data terbaru harusnya berada paling atas ⏬ 📅 2025-10-31 ✅ 2025-10-30
- [x] ➕ 2025-10-29 input field pilih no reservasi kalau tujuannya gada reservasinya 🆔 vt3yz6 ✅ 2025-10-30
- [x] ➕ 2025-10-30 verification popup ketika ingin kembali dari detail daftar order 🔽 📅 2025-10-31 ✅ 2025-10-30
- [x] ➕ 2025-10-30 pada halaman operator: data truck, driver & incoming material harusnya tidak boleh duplikat, tambahkan validasi 🔼 ✅ 2025-10-30
- [x] ➕ 2025-11-03 DITAMBAHKAN TOTAL PALLET & FREEZE 🔽 📅 2025-11-04 ✅ 2025-11-05
- [x] ➕ 2025-11-03 fixed bug header layout /tms/master-control/adm-wsm/input-nti sudah & belum detail modal 🔽 ✅ 2025-11-03
- [x] ➕ 2025-11-05 create order outgoing pada halaman pilih material buat table 2 kolom: pilih dan material (data yang diperlukan) 📅 2025-11-05 ✅ 2025-11-05
- [x] ➕ 2025-11-05 Tambahin search field, lalu ketika di search list material & material yang dipilih juga ikut terfilter ⏫ 📅 2025-11-05 ✅ 2025-11-05
- [x] ➕ 2025-11-05 fix bug pilih no reservasi when user create order 🔼 📅 2025-11-05 ✅ 2025-11-05
- [x] ➕ 2025-11-14 buat select no.reservasi di create order pengawas menggunakan selectize 🔼 📅 2025-11-17 ✅ 2025-11-14
- [x] ➕ 2025-11-14  pada halaman pengawas request order untuk outgoing ketika ada multiple no reservasi serta diklik salah satunya maka memunculkan list material dari no reservasi tersebut 🔼 📅 2025-11-23 ✅ 2025-11-21
- [x] ➕ 2025-11-14 create button to switch camera for driver 🔼 📅 2025-11-21 ✅ 2025-11-20
- [x] ➕ 2025-11-14 buat list order dari driver desc 🔽 📅 2025-11-21 ✅ 2025-11-20
- [x] ➕ 2025-11-17 tiap material ditambahkan note di my picking 🔼  list 📅 2025-11-21 ✅ 2025-11-19
- [x] ➕ 2025-11-17 disabled edit qty in my picking list 🔼 📅 2025-11-21 ✅ 2025-11-19
- [x] ➕ 2025-11-17  sisa dari pilihan material ketika di delete masih bisa milih reservasinya ⏫ 📅 2025-11-20 ✅ 2025-11-20
- [x] ➕ 2025-11-17 ketika pengawas maupun driver scan qr setelah berhasil kemudian diarahkan ke halaman login kembali, besar kemungkinannya karena beda url yang diakses pertama kali dan setelah scan ✅ 2025-11-19
- [x] ➕ 2025-11-17 perbaiki tampilan nomer NTI di table halaman delivery confirmation serta di NTI 🔼 📅 2025-11-21 ✅ 2025-11-19
- [x] ➕ 2025-11-17 detail muatan barang di driver buat responsive ⏬ 📅 2025-11-21 ✅ 2025-11-19
- [x] ➕ 2025-11-24 pengawas order request, list material dan pilihan material diubah user interfacenya agar lebih user friendly 🔼 📅 2025-11-24 ✅ 2025-11-24
- [x] ➕ 2025-11-26 pengawas request order outgoing ketika membuat request lalu memilih semua material di no reservasi kemudian di delete, ketika mau pilih lagi no reservasi tersebut malah menghilang 📅 2025-11-27 ✅ 2025-11-26
- [x] ➕ 2025-11-26 fix bug list driver order 🔼 📅 2025-11-26 ✅ 2025-11-26
- [x] ➕ 2025-11-26 display NTI column in delivery confirmation & inconsisten detail data 🔼 📅 2025-11-28 ✅ 2025-11-27
- [x] ➕ 2025-11-26 update ui of driver list order 🔽 📅 2025-12-02 ✅ 2025-12-01
- [x] ➕ 2025-11-26 bug driver pulang kosongan 🔼 📅 2025-11-28 ✅ 2025-12-01
- [x] ➕ 2025-11-27 button switch camera on driver 🔽 📅 2025-12-02 ✅ 2025-12-01
- [x] ➕ 2025-11-26 report outgoing descending for all headers 🔽 📅 2025-12-01 ✅ 2025-11-27
- [x] ➕ 2025-12-01 fix bug memastikan semua no reservasi dapat dibuat menjadi order ⏫ 📅 2025-12-02 ✅ 2025-12-02
- [x] ➕ 2025-12-02 list descending untuk delivery confirmation dan input nti 🔼 📅 2025-12-02 ✅ 2025-12-02
- [x] ➕ 2025-12-02 trial tms fase outgoing ⏳ 2025-12-04 ✅ 2025-12-09
- [x] ➕ 2025-12-03 combine material di my picking list ⏫  ✅ 2025-12-10
	- salah satu pendeketan yang bisa dipakai itu tambah column baru untuk identifier, ambil dari mid nya kemudian increment postfix
	- jadi quantity nya yang diambil berdasarkan identifier yang telah dibuat
	- total pallet juga ikut berubah
	- lalu secara ui terdapat button buat liat detail material yang di combine tersebut, artinya ketika material (receh) tersebut digabung oleh adm wsm di my picking list maka material yang dipilih itu menjadi 1 baris saja
	- update tampilan modal detail material di picking list staging
	- update tampilan modal detail material di pengawas daftar order 
	- update tampilan modal detail list request order di pengawas order
	- update tampilan detail material di order list driver
- [x] ➕ 2025-12-03 multiple tujuan untuk outgoing-tujuan N1 & tujuan N2 🔼 ✅ 2025-12-15
	- tujuan menggunakan selectize agar bisa multiple input
	- max 3 tujuan
	- delivery confirmation: di detail hanya menampilkan material departemen yang bersangkutan
	-  untuk inisiasi nti: tetep adm wsm isi seperti biasa
- [x] ➕ 2025-12-04 buat search bar diatas dari selectize; biar berjalan di backend pakai ajax; free text ini dia akan filter nama material di dalam no reservasi sesuai dengan apa yang dia ketik 🔼 📅 2025-12-11 ✅ 2025-12-08
- [x] ➕ 2025-12-04 di my picking list: ✅ 2025-12-09
	- kolom request by hidden
	- modal detail full satu layer
	- kolom to dummy hide
	- header jangan freeze
- [x] ➕ 2025-12-04 buat default only selected wsm untuk outgoing asal ⏬  📅 2025-12-10 ✅ 2025-12-09
- [x] ➕ 2025-12-07 meeting bersama mas rizal di warehouse membahas fitur: combine material; multiple tujuan; search bar 🔼 📅 2025-12-08 ✅ 2025-12-08
- [x] ➕ 2025-12-09 delivery confirmation group by mid nya jadi berurutan gitu 🔽  ✅ 2025-12-10
- [x] ➕ 2025-12-11 kalau user salah combine material, cara pecah balikin seperti semula gimana?? 🔼 ✅ 2025-12-12
- [x] ➕ 2025-12-16 ubah keterangan di pengawas request order serta di driver order list agar ketika SJ tersebut multiple maka menampilkan tujuannya sesuai dengan yang dipilih pengawas 🔼 📅 2025-12-18 ✅ 2025-12-16
- [x] ➕ 2025-12-16 delivery confirmation sort berdasarkan MID 🔽 📅 2025-12-18 ✅ 2025-12-16
- [x] ➕ 2025-12-16 refactor ui/ux & logic detail DO agar dapat menerima multiple tujuan 🔼 📅 2025-12-18 ✅ 2025-12-16
- [x] ➕ 2025-12-17 fix user produksi picking list getdata sesuai sama yang adm wsm picking list upload 🔼 📅 2025-12-17 ✅ 2025-12-17
- [x] ➕ 2025-12-18 penambahan total pallet di history user produksi 🔽 📅 2025-12-19 ✅ 2025-12-18
- [x] ➕ 2025-12-18 di report outgoing picking list buat detail material untuk combine 🔼 📅 2025-12-19 ✅ 2025-12-18
- [x] ➕ 2025-12-18 fix quantity total pallet adm wsm dokumen reservasi 🔼 📅 2025-12-18 ✅ 2025-12-18
- [x] ➕ 2025-12-20 ada salah logika pengurangan di saat picking list UploadDokumenReservasiWsm.php, dimana harusnya ga exactly sama, biar di round up aja... jadinya nilai pallet 0.04 (mana ada pallet 0.) ngaruh ke history juga coy - user produksi 🔼 📅 2025-12-22 ✅ 2025-12-20
- [x] ➕ 2025-12-22 button delete di operator mid master data material 🔽 📅 2025-12-23 ✅ 2025-12-22
- [x] ➕ 2025-12-24 mengkonfigurasikan complaint: ✅ 2025-12-26
	- kondisi ketika produksi complain 2 material (A & B)
	- material A & B masuk ke adm wsm lalu A di approve serta B di reject
	- A hilang di adm wsm complaint lalu material A masuk ke staging
	- B reject maka kembali ke delivery confirmation
- [x] ➕ 2025-12-29 fix bug, material yang sudah di complaint kemudian di approve oleh adm wsm, lalu dikirim kembali, setelah selesai dikirim oleh driver malah langsung masuk ke complaint, harusnya ke delivery confirmation lagi 🔼 📅 2025-12-30 ✅ 2025-12-30
- [x] ➕ 2026-01-05 trial tms outgoing di lapangan ⏫ ⏳ 2026-01-06 ✅ 2026-01-08
- [x] ➕ 2026-01-05 sesuaikan detail reporting agar sesuai dengan permintaan mas rizal![[IMG/Pasted image 20260105144231.png]] ✅ 2026-01-07
- [x] ➕ 2026-01-08 update user experience dari pengawas staging. Outgoing langsung munculin material tanpa harus melalui pilih reservasi 🔼  ✅ 2026-01-09
- [x] ➕ 2026-01-09 di produksi confirm material buat material yang tampil itu kalau midnya sama akumulasi total pallet mid tersebut 🔽 📅 2026-01-12 ✅ 2026-01-12
- [x] ➕ 2026-01-09 produksi confirm material, ketika semua material di confirm maka tidak mengunggah foto, tetapi ketika DO tersebut material yang dipilih tidak semua di ceklis, memunculkan unggahan foto. Untuk complain masih sama 🔽 📅 2026-01-12 ✅ 2026-01-12

## Pending Tasks
- [x] ➕ 2025-10-31 JIKA SJ SUDAH TERBENTUK (NO RESERVASI YANG DIPILIH CONTOH : 05, 06, 07) DAN PENGAWAS SALAH PILIH MATERIAL, KEMUDIAN DI DELETED SJ TERSEBUT DAN BERHASIL, SAAT PENGAWAS INGIN PILIH NO RESERVASI TERSERBUT, MATERIALNYA SUDAH HILANG SEMUA 🔼 ✅ 2025-11-
- [ ] ➕ 2025-12-10 update button show detail material combined di pengawas & driver agar badge nya yang bertuliskan "DETAIL" saja yang jadi toogle 🔽
- [ ] ➕ 2025-12-18 material yang sudah di combine kemudian di combine lagi, lalu juga ngepecahnya 🔼 

#### Outgoing Material
> adm produksi upload material
> adm wsm upload material 


## Drawing
[[]]
