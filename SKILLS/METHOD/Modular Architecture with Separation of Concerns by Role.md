#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Modular Architecture|Modular Architecture]]
- [[#Konsep Dasar|Konsep Dasar]]
	- [[#Konsep Dasar#1. Modul|1. Modul]]
	- [[#Konsep Dasar#2. Concern|2. Concern]]
	- [[#Konsep Dasar#3. Role|3. Role]]

## Introduction
Ada case dimana gue 2x refactoring codebase dimana gue dapet project dari PT.PAS (TMS) dan gue nemu 1 istilah yg menarik untuk gue bahas "Modular Architecture with Separation of Concerns by Role".

## Modular Architecture
- Arsitektur perangkat lunak yang membagi aplikasi menjadi **modul-modul kecil** yang **saling terpisah** dan **dapat digunakan kembali**.
- Setiap modul memiliki **fungsi spesifik** dan **tanggung jawab sendiri**.
- Tujuan: **scalability**, **maintainability**, dan **pengembangan tim** yang lebih efisien.

## Konsep Dasar

### 1. Modul
- Bagian aplikasi yang memiliki **fungsi spesifik**.
- Contoh: Modul User, Modul Product, Modul Order, dll.

### 2. Concern
- Bagian kode yang **bertanggung jawab** atas aspek tertentu.
- Contoh: Business Logic, Data Access, Presentation, Authentication, dll.

### 3. Role
- Jenis pengguna dengan **hak akses dan tanggung jawab** berbeda.
- Contoh: Admin, Customer, Manager, dll.

```
project/
├── app/
│   ├── Modules/
│   │   ├── User/
│   │   │   ├── Controllers/
│   │   │   │   ├── Api/
│   │   │   │   │   ├── Admin/
│   │   │   │   │   └── User/
│   │   │   │   └── Web/
│   │   │   │       ├── Admin/
│   │   │   │       └── User/
│   │   │   ├── Models/
│   │   │   └── Routes/
│   │   │       ├── api.php
│   │   │       └── web.php
│   │   ├── Product/
│   │   │   ├── Controllers/
│   │   │   │   ├── Api/
│   │   │   │   │   ├── Admin/
│   │   │   │   │   └── Guest/
│   │   │   │   └── Web/
│   │   │   │       ├── Admin/
│   │   │       └── Guest/
│   │   │   ├── Models/
│   │   │   └── Routes/
│   │   │       ├── api.php
│   │   │       └── web.php
│   │   └── Order/
│   │       ├── Controllers/
│   │       ├── Models/
│   │       └── Routes/
│   └── Core/
│       └── Models/
├── resources/
│   └── views/
│       └── modules/
│           ├── user/
│           │   ├── admin/
│           │   │   ├── index.blade.php
│           │   │   ├── create.blade.php
│           │   │   └── edit.blade.php
│           │   └── user/
│           │       ├── profile.blade.php
│           │       └── dashboard.blade.php
│           ├── product/
│           │   ├── admin/
│           │   │   ├── index.blade.php
│           │   │   └── create.blade.php
│           │   └── guest/
│           │       └── list.blade.php
│           └── order/
│               ├── admin/
│               │   └── index.blade.php
│               └── user/
│                   └── my-orders.blade.php
└── routes/
    ├── api.php
    └── web.php
```

Date: 06-10-2025