#pas 

# 📝 **CATATAN FLOW BISNIS APLIKASI SMART-LAB**
---
## **1. FLOWCHART: SMART LAB - USER (Gambar 2)**
![[IMG/User_Smartlab_Diagram.drawio 1.png]]
### 🎯 **Tujuan**

Menggambarkan alur navigasi awal pengguna setelah login, memilih departemen, dan memilih jenis aktivitas (harian atau permintaan analisis).

### 🔁 **Alur Proses**

1. **Start:** Pengguna membuka aplikasi.
    
2. **Pilih Departemen (`PILIH DEPT`):**
    - Setelah login sukses, pengguna diarahkan ke menu utama untuk memilih departemen.
3. **Cabang Utama:**
    - **`SAMPLE DAILY ACTIVITY`**: Untuk pencatatan sampel rutin harian.
    - **`SAMPLE PERMINTAAN ANALISA`**: Untuk permintaan analisis khusus/non-rutin.
4. **Sub-Cabang Berdasarkan Departemen:**
    - Di bawah `SAMPLE DAILY ACTIVITY`:
        - **KIMIA**: Mengarah ke berbagai jenis sampel kimia (Cleaning Fryer, Air/Alkali/Tepung, Mie Premix, Mie Basah, Adonan, Logam, AV/FC/KA).
        - **MIKROBIOLOGI**: Mengarah ke `SAMPLE MIE PACKING MIKRO`.
    - Di bawah `SAMPLE PERMINTAAN ANALISA`:
        - **KIMIA**: Mengarah ke form umum "ISI FORM".
        - **MIKROBIOLOGI**: Mengarah ke form umum "ISI FORM".
5. **Endpoint:**
    - Semua jalur akhirnya mengarah ke **`ISI FORM`**, yang merupakan titik awal input data detail untuk setiap jenis sampel.

### 📌 **Catatan Penting**

- Flowchart ini bersifat **high-level** dan menunjukkan struktur navigasi, bukan detail proses input.
- Tidak ada validasi atau loop, hanya menunjukkan pilihan menu.
- Semua jenis sampel di `SAMPLE DAILY ACTIVITY` akan mengarah ke form input yang spesifik, meskipun digambarkan sebagai satu blok "ISI FORM" untuk kesederhanaan.
- user pada saat mengisi daily activity > kimia > sample av;fc;ka haruslah tidak boleh ada line yang kosong, 
  1. jika terdapat line yang kosong setelah inisiasi data, maka datanya tidak dikirim
  2. jika casenya seperti: line 1 isi; line 2 kosong; line 3 isi, maka akan mengirim line 1 saja. haruslah mengisi line 2 baru terkirim semua data line

---

## **2. FLOWCHART: NOODLE 1 (Gambar 4)**
 ![[IMG/DA_-_Lab_Online_Noodle_1_(1)-Create Sample.drawio 1.png]]
### 🎯 **Tujuan**

Menggambarkan alur proses bisnis secara **detail dan teknis** untuk **Sample Daily Activity** dan **Sample Permintaan Analisa**, termasuk validasi dan pengecekan kelengkapan data.

### 🔁 **Alur Proses**

#### **A. Start & Login**

1. **Start:** Mulai aplikasi.
2. **Input Username & Password:** Masukkan kredensial.
3. **Validasi Login:**
    - Jika **salah**, kembali ke input username/password.
    - Jika **benar**, lanjut ke `Pilih Dept (Noodle 1)`.

#### **B. Pilih Departemen & Aktivitas**

1. **Pilih Dept (Noodle 1):** Pilih antara `Sample Daily Activity` atau `Sample Permintaan Analisa`.

---

### **C. Sub-Alur: Sample Daily Activity**

#### **C.1. Pilih Kimia → Pilih Packing / Pilih Proses**

- **Jalur Packing:**
    - `Sample AV, FC, dan KA` → Form Create → Decision `Create Lengkap?`
        - **NO** → Kembali ke form Create.
        - **YES** → Selesai, arah ke `END (Ke Input Analis)`.
- **Jalur Proses:**
    - `Sample Logam`, `Sample Adonan`, `Sample Mie Basah`, `Sample Premix`, `Sample Air Bahan`, `Sample Cleaning Fryer` → Masing-masing memiliki form Create → Decision `Create Lengkap?`
        - **NO** → Kembali ke form Create.
        - **YES** → Selesai, arah ke `END (Ke Input Analis)`.

#### **C.2. Pilih Mikrobiologi**

- `Mie Packing Mikro` → Form Create → Decision `Create Lengkap?`
    - **NO** → Kembali ke form Create.
    - **YES** → Selesai, arah ke `END (Ke Input Analis)`.

---

### **D. Sub-Alur: Sample Permintaan Analisa**

#### **D.1. Pilih Kimia**

- Form Create (Tanggal, Dept, Nama Sample, Jumlah, Keperluan, Parameter Analisis) → Decision `Create Lengkap?`
    - **NO** → Kembali ke form Create.
    - **YES** → Selesai, arah ke `END (Approval Request PA)`.

#### **D.2. Pilih Mikrobiologi**

- Form Create (Tanggal, Dept, Nama Sample, Jumlah, Keperluan, Parameter Analisis) → Decision `Create Lengkap?`
    - **NO** → Kembali ke form Create.
    - **YES** → Selesai, arah ke `END (Approval Request PA)`.

---

### 📌 **Catatan Penting (Noodle 1)**

- **Decision `Create Lengkap?`** adalah titik validasi penting. Jika data tidak lengkap, sistem akan meminta user untuk melengkapi.
- Semua proses **Daily Activity** berakhir dengan `END (Ke Input Analis)`, artinya data siap untuk tahap pengujian oleh tim analis.
- Semua proses **Permintaan Analisa** berakhir dengan `END (Approval Request PA)`, artinya permintaan harus disetujui sebelum proses pengambilan sampel dimulai.
- Tidak ada perbedaan signifikan antara Noodle 1 dan Noodle 2 dalam hal logika, hanya perbedaan tata letak visual.

---

## **3. FLOWCHART: NOODLE 2 (Gambar 3)**
![[IMG/DA_-_Lab_Online_Noodle_2-Create Sample.drawio 1.png]]
### 🎯 **Tujuan**

Sama seperti Noodle 1, namun dengan **tata letak visual yang berbeda** untuk memudahkan pembacaan atau presentasi. Isi logika proses identik.

### 🔁 **Alur Proses**

> _(Struktur proses sama persis dengan Noodle 1, hanya layout diagram yang berbeda)_

#### **A. Start & Login**

Identik dengan Noodle 1.

#### **B. Pilih Departemen & Aktivitas**

Identik dengan Noodle 1.

---

### **C. Sub-Alur: Sample Daily Activity**

#### **C.1. Pilih Kimia → Pilih Packing / Pilih Proses**

- **Jalur Packing:**
    - `Sample AV, FC, dan KA` → Form Create → Decision `Create Lengkap?` → Loop jika NO, End jika YES.
- **Jalur Proses:**
    - `Sample Logam`, `Sample Adonan`, `Sample Mie Basah`, `Sample Premix`, `Sample Air Bahan`, `Sample Cleaning Fryer` → Form Create → Decision `Create Lengkap?` → Loop jika NO, End jika YES.

#### **C.2. Pilih Mikrobiologi**

- `Mie Packing Mikro` → Form Create → Decision `Create Lengkap?` → Loop jika NO, End jika YES.

---

### **D. Sub-Alur: Sample Permintaan Analisa**

#### **D.1. Pilih Kimia**

- Form Create → Decision `Create Lengkap?` → Loop jika NO, End jika YES → `END (Approval Request PA)`.

#### **D.2. Pilih Mikrobiologi**

- Form Create → Decision `Create Lengkap?` → Loop jika NO, End jika YES → `END (Approval Request PA)`.

---

### 📌 **Catatan Penting (Noodle 2)**

- **Logika proses identik dengan Noodle 1.** Perbedaan hanya pada **layout dan penyajian visual**.
- Diagram ini mungkin lebih cocok untuk presentasi karena struktur cabangnya lebih vertikal dan jelas.
- Semua decision point dan endpoint tetap sama, menjaga konsistensi logika bisnis.
- Digunakan untuk memastikan bahwa semua pengguna memahami alur proses dari sudut pandang yang berbeda.


# TASK
- [x] ➕ 2025-11-07 di halaman history permintaaan, pada section "History Permintaan" ketika di klik lihat selengkapnya maka memunculkan modal detail, ketika di klik fotonya lebih baik muncul gambar berupa modal saja 🔽 📅 2025-11-18 ✅ 2025-11-11
- [x] ➕ 2025-11-07 tambahkan badge informasi mengenai banyaknya permintaan, buat sebagai card 🔼 📅 2025-11-11 ✅ 2025-11-10
- [x] ➕ 2025-11-07 pada /user/monitoring-ticket pa, ubah kolom jenis analisis menjadi departemen 🔼 📅 2025-11-11 ✅ 2025-11-10
- [x] ➕ 2025-11-07 pada /smart-lab/user/noodle-1/permintaan-analisis kimia maupun mikrobiologi, pada section **history permintaan** tambahkan button download pdf dengan kondisi hanya muncul ketika hasil analisis sudah approve 🔽 📅 2025-11-11 ✅ 2025-11-10
- [x] ➕ 2025-11-07 /smart-lab/monitoring-ticket/pa-kimia/18 beri footer pada halaman pdf 🔽 📅 2025-11-12 ✅ 2025-11-10
- [x] ➕ 2025-11-10 ubah logic jika 1 nomor doc punya 3 sample lalu 1 diantaranya ada yang reject maka seluruhnya reject, kalau ada 1 on progress dan 2 lainnya closed maka status tetap on progress, hanya closed ketika semua sample sudah closed ⏫ 📅 2025-11-12 ✅ 2025-11-11
- [x] ➕ 2025-11-11 fix bug logic controller di controller approval ⏫  📅 2025-11-12 ✅ 2025-11-11
- [x] ➕ 2025-11-10 ubah layout detail hasil analisis, pada satu sample langsung menampilkan detail nilai dari parameternya, langsung dikelompokkan gitu 🔼 📅 2025-11-12 ✅ 2025-11-12
- [x] ➕ 2025-11-12 mengubah layout detail hasil pada user create permintaan analisis kimia maupun mikrobiologi 🔼 📅 2025-11-12 ✅ 2025-11-12
- [x] ➕ 2025-11-11 membuat badge count di halaman approval maupun qa untuk departemen yang merepresentasikan banyaknya data di tiap departemen 🔼 📅 2025-11-18 ✅ 2025-11-19
- [x] ➕ 2025-11-13 update refactor name of status card in montioring ticket & user create pa 🔼 📅 2025-11-13 ✅ 2025-11-13
- [x] ➕ 2025-11-13 fix bug data pdf layout duplicate 🔼 📅 2025-11-13 ✅ 2025-11-13
- [x] ➕ 2025-11-18 mengubah style halaman approval qa serta user 🔽 📅 2025-11-19 ✅ 2025-11-19
- [x] ➕ 2025-11-20 perbaiki schedule di permintaan analisis agar yang ditampilin itu sesuai, seperti yang di daily activity 🔼 📅 2025-11-25 ✅ 2025-11-20
- [x] ➕ 2025-11-20 buat flow alur penggunaan smart_lab untuk user saja 🔼 📅 2025-11-23 ✅ 2025-11-27
- [x] ➕ 2025-11-21 buat report seperti di lab online untuk approval permintaan analisis kimia serta biologi 🔼 📅 2025-11-26 :  ![[IMG/screencapture-172-21-8-240-lab-online-public-sample-minyak-report-sample-2025-11-21-10_02_01.png]] ✅ 2025-11-25
	- kolom: nama requestor; nama sample; parameter; keterangan; tanggal;
	- filter tanggal dari sampai
	- button export excel, untuk export itu harus detail:
	      - PA_KIMIA: no_doc; nama requestor; requestor tanggal (request_at);  keperluan uji; dept
	      - PA_KIMIA_SAMPLE: jumlah sample; approve receive; simpan hasil analisis oleh; approve hasil
	      - PA_KIMIA_PARAMETER: hasil analisis;
- [x] ➕ 2025-11-25 button DA selain noodle disable 🔽 📅 2025-11-25 ✅ 2025-11-25
- [x] ➕ 2025-11-25 keterangan multiple input berdasarkan banyaknya sample ⏫ 📅 2025-11-27 ✅ 2025-11-26
- [x] ➕ 2025-11-25 di excel hasil report tambahkan status 🔽 📅 2025-11-30 ✅ 2025-11-26
- [x] ➕ 2025-11-25 sosialisasi penggunaan smart lab PA untuk user ⏳ 2025-11-28 ✅ 2025-11-28
- [x] ➕ 2025-12-01 plotting grup qa sesuai role agar menu aplikasi sesuai dengan group nya ⏫ 📅 2025-12-02 ![[IMG/Pasted image 20251201154225.png]]![[Files/Nama Analis.xls]] ✅ 2025-12-02
- [x] ➕ 2025-12-28 pembuatan skala prioritas per permintaan analisis 🔼 📅 2025-12-04 ✅ 2025-12-03
- [x] ➕ 2025-12-28 membuat feature "Confidential", yang artinya yang dapat melihat PA tersebut hanya yang user membuat saja dan team QA 🔼 📅 2025-12-05 ✅ 2025-12-03
- [x] 2025-11-28 revisi sample PA, implementasinya buat berupa button edit dan hapus sample di halaman user 🔼 📅 2025-12-04 ✅ 2025-12-04
- [x] ➕ 2025-12-02 membuatkan akun pas untuk user berikut: 🔼 📅 2025-12-02 ![[IMG/Screenshot from 2025-12-02 09-56-59.png]] ✅ 2025-12-02
- [x] ➕ 2025-12-04 ketika approve hasil PA, ditambahkan note; free text; nullable, yang bakal muncul di pdf nya 🔼 📅 2025-12-09 ✅ 2025-12-05
- [x] ➕ 2025-12-04 untuk monitoring ticket get semua data-semua user dapat lihat, hanya saja untuk data yang 'confidential' user yang tidak bersangkutan tidak bisa lihat detail (ikon mata); status dihilangkan; pdf dihilangkan ⏫  ✅ 2025-12-05
- [x] ➕ 2025-12-08 refactor tampilan data di monitoring ticket agar yang urgent dengan status finish atau rejected berada pada urutan yang seharusnya sesuai dengan created_at 🔼 📅 2025-12-09 ✅ 2025-12-09
- [x] ➕ 2025-12-13 parameter di mikrobiologi dapat menerima teks input bukan hanya number 🔼 📅 2025-12-13 ✅ 2025-12-13
- [x] ➕ 2025-12-10 improve style of monitoring ticket PA kimia & mikrobiologi agar lebih baik 🔼 📅 2025-12-11 ✅ 2025-12-11
- [x] ➕ 2025-12-12 fitur upload foto ketika approve hasil PA yang akan ditampilkan di pdf 🔼 📅 2025-12-14 ✅ 2025-12-15
- [x] ➕ 2025-12-16 ubah nama approval di pdf menjadi user yang approve hasil PA bukan receive 🔽 📅 2025-12-17 ✅ 2025-12-16
- [x] ➕ 2025-12-17 yang bisa kirim notif ke tele: 🔼 📅 2025-12-19 ✅ 2025-12-18
	- approve receive
	- reject receizve + alasan
	- approve hasil
	- request sample dari user![[IMG/Pasted image 20251217171420.png]]
- [x] ➕ 2025-12-17 buat modal keterangan ketika reject PA receive, lalu keterangan reject tersebut muncul di result monitoring ticket atau detail create PA user 🔽 📅 2025-12-19 ✅ 2025-12-18
- [x] ➕ 2025-12-18 urutin export excel berdasarkan create at & tambahin note dari keterangan di table PA kimia  🔽 📅 2025-12-19 ✅ 2025-12-19
- [x] ➕ 2025-12-30 di notif tele untuk approval hasil analisis ditambahin no dok, dept, sama requestor  🔽 ✅ 2025-12-30
- [x] ➕ 2025-12-30 di notif tele untuk sample yang private detail samplingnya di hide saja 🔽  📅 2025-12-31 ✅ 2025-12-30
      ![[IMG/Screenshot from 2025-12-30 14-32-49.png]]
- [x] ➕ 2026-01-12 disable notif when result of PA was rejected 🔽 📅 2026-01-12 ✅ 2026-01-12
- [x] ➕ 2026-01-11 disable chat on telegram 🔽 📅 2026-01-13 ✅ 2026-01-12
- [x] gawi edit sample ✅ 2026-05-21

### Pending
- [x] ➕ 2025-11-11 halaman /qa/ ketika ingin input nilai parameter dibuat agar request postnya berjalan dibelakang, di view nya kasih field untuk input nilai yang parameternya di klik, begitu enter atau keluar dari field tersebut maka save dari segi backendnya ⏫ ✅ 2025-12-10
- [x] ➕ 2026-07-29 lab online all-line PRN perbaiki ✅ 2026-07-29
- ![[Pasted image 20260729163818.png]]

## Lab Eksternal
### User
- user pilih lab eksternal
- ada dua pilihan: ETO & Regulasi
	1. ETO:
	   sample RM Incoming
	   sample RM PSS
	   Sample Mie Packing Export
	   Sample Mie Packing Lokal
	2. Regulasi:
	   SNI / Regulasi
	   By Request
- Sample Mie Packing
  1. untuk sample mie packing kolom kode varian isinya adalah alias dari nama sample, jadi ketika alias dipilih maka otomatis sinkronisasi nama sample
  2. tanggal produksi tampilin tanggal hari sample tersebut dibuat tapi di database timestamp ajaa, karena akan mengambil tanggal sample tersebut dibuat
- Tanggal kedatangan RM Incoming & RM PSS
### Admin
1. buat sub-menu di smart-lab untuk admin
2. buar permission untuk admin & purchasing di smart lab
3. departemen sama seperti dari qa | user | approval tetapi disable semua hanya lab external saja
4. buat navigasinya itu ditumpuk aja dari satu modal ke modal lain, seperti halnya yang di qa
5. buat rm incoming; rm pss; mie export; mie local, tambah kolom disebelah dari vendor untuk upload dokumen

1 no penawaran itu bisa punya beberapa sample, yang input no penawaran itu gawi. Gawi itu QA PT PAS yang kerja diluar pt pas-dinas lah ya gampangnya.

flownya gini kurang lebih:
1. start
2. user create sample lab external
3. admin "receive sample"
4. admin input lengkap kemudian pilih vendor tujuan: SGS; BVI; SIG, sekaligus upload dokumen
5. masuk ke gawi, gawi milih sample mana aja-diambil dari jenis permintaan-yang mau dijadiin 1 no penawaran, lalu input no penawaran & upload doc
6. abis dari gawi ke admin lagi buat si admin bikin PR, dari no penawaran masing masing vendor, input lah no PR
7. baru abis itu ke purchasing, input no PO, update status nya (Wait atau Release)
8. end 

### Meeting 19 feb 2026
1. tambah flow: 
   - setelah dari proses PR 
   - kemudian balik lagi ke admin QA untuk isi No. IM GR-tanggal GR di create ketika submit
   - kemudian setelahnya ke gawi lagi yang memutuskan sesuai atau tidak sesuai, berarti gawi ni punya halaman yang isinya 2 path mau ke input nomor penawaran upload dokumen atau yang beri sesuai tidak sesuai dari no po tersebut
2. dari purchasing dia ga hanya isi po tapi juga ada disposisi release/wait; jadi ada kemungkinan inisiasi no po di hari ini besoknya baru release
3. di monitoring ticket, itu dibuat per vendor aja... tapi yaa isinya sesuai sama vendor, sebetulnya mirip kayak gawi sih... cuma ga semuanya di tampilin hanya yang sekiranya penting pentingnya aja... kalau di gawi itu persample, di monitoring ticket ini dia per no penawaran, jadi buat liat detail sample nya apa aja buat nomor penawaran tersebut
4. buat grup telegram bot terpisah untuk lab eksternal.
5. untuk jenis sample eto semuanya: rm-pss; mie export; mie lokal; seasoning itu tanggal kedatangan diambil per hari ini terus fieldnya di disable, lalu ada  field tambahan tanggal produksi serta tanggal expired sama halnya kayak yang di rm-incoming