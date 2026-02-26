#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Apa Itu RBAC?|Apa Itu RBAC?]]
- [[#Konsep Dasar RBAC|Konsep Dasar RBAC]]
	- [[#Konsep Dasar RBAC#1. User (Pengguna) - Wajib|1. User (Pengguna) - Wajib]]
	- [[#Konsep Dasar RBAC#2. Role (Peran) - Wajib|2. Role (Peran) - Wajib]]
	- [[#Konsep Dasar RBAC#3. Permission (Izin) - Wajib|3. Permission (Izin) - Wajib]]
	- [[#Konsep Dasar RBAC#4. Operation (Operasi) - Opsional|4. Operation (Operasi) - Opsional]]
	- [[#Konsep Dasar RBAC#5. Object (Objek) - Opsional|5. Object (Objek) - Opsional]]
- [[#Keuntungan RBAC|Keuntungan RBAC]]
- [[#Contoh RBAC|Contoh RBAC]]

## Introduction
Ada case dimana gue interview di tempat lain dan menemukan istilah RBAC (Role Based Access Control). Yuk gas bahas RBAC!

## Apa Itu RBAC?
**Role Based Access Control (RBAC)** adalah model manajemen akses keamanan yang **mengatur hak akses pengguna berdasarkan peran (role)** yang mereka miliki dalam suatu sistem. Dalam RBAC, **bukan setiap pengguna secara individu** yang diberi izin, melainkan **peran** yang didefinisikan terlebih dahulu, dan pengguna ditempatkan dalam peran tersebut.

## Konsep Dasar RBAC
Model RBAC terdiri dari beberapa komponen utama:

### 1. User (Pengguna) - Wajib
- Individu atau entitas yang menggunakan sistem.
- Contoh: admin, staf, manajer, dll.

### 2. Role (Peran) - Wajib
- Kumpulan izin atau hak akses yang terkait dengan tanggung jawab tertentu.
- Contoh: role "Admin", "Editor", "Viewer", dll.

### 3. Permission (Izin) - Wajib
- Hak akses spesifik terhadap objek atau fungsi dalam sistem.
- Contoh: "read", "write", "delete", "execute", dll.

### 4. Operation (Operasi) - Opsional
- Aksi yang bisa dilakukan oleh pengguna.
- Contoh: menambah data, menghapus file, mengakses laporan.

### 5. Object (Objek) - Opsional
- Sumber daya yang dilindungi, seperti file, database, atau layanan.
- Contoh: dokumen, tabel, layanan API.

## Keuntungan RBAC
- **Manajemen akses lebih mudah**: Tidak perlu mengatur izin satu per satu untuk setiap pengguna.
- **Skalabilitas tinggi**: Cocok untuk sistem dengan banyak pengguna.
- **Pemisahan tanggung jawab**: Mencegah akses yang tidak seharusnya.
- **Audit dan kepatuhan**: Lebih mudah untuk meninjau dan mengelola hak akses.

## Contoh RBAC

![[Pasted image 20251006162747.png]]

![[Pasted image 20251006162804.png]]

Setiap group ada auth permission nya. Misal group foreman_ite ada permission untuk ke halaman atau fitur lain. - Portal MyPas Online

Date: 06-10-2025