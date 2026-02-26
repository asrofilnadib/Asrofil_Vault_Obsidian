#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Konfigurasi|Konfigurasi]]
- [[#Npm|Npm]]

## Introduction
Ketika saya mendapatkan project CBT, saya mengalami kesulitan di developmentnya. Ketika ada perubahan di frontend maupun backend maka harus di refresh terlebih dahulu. Jelas ini tidak efisien dalam segi waktu. Sebelumnya saya sempat menggunakan Laravel Mix namun di Laravel 8 keatas mereka menggunakan ViteJS.

Intinya Vitejs itu memudahkan kita untuk develop frontend. Berikut fitur nya berdasarkan ChatGPT
1. **Development Server yang Cepat**: ViteJS menyediakan server pengembangan yang sangat cepat dengan kemampuan memuat halaman web secara instan (instant page load) dan kompilasi kode yang sangat cepat.
2. **HMR (Hot Module Replacement)**: ViteJS mendukung Hot Module Replacement, yang memungkinkan pengembang untuk melihat perubahan dalam kode mereka secara langsung tanpa perlu menyegarkan halaman browser.
3. **Optimized Build for Production**: Selain digunakan di lingkungan pengembangan, ViteJS juga dapat menghasilkan build produksi yang dioptimalkan untuk kinerja dan ukuran yang lebih kecil.
4. **Native ESM (EcmaScript Modules) Support**: ViteJS memanfaatkan fitur-fitur baru dalam spesifikasi EcmaScript Modules (ESM), yang memungkinkan penggunaan modul JavaScript secara native di browser tanpa perlu transpilasi atau bundling.
5. **Plugin System**: ViteJS memiliki sistem plugin yang kuat yang memungkinkan pengguna untuk menyesuaikan dan memperluas fungsionalitasnya sesuai kebutuhan proyek.
6. **Dukungan untuk Vue.js dan React**: ViteJS memiliki dukungan bawaan untuk kerangka kerja Vue.js dan React, memungkinkan pengembang untuk memulai proyek dengan cepat menggunakan kerangka kerja favorit mereka.
7. **Paket Pilihan**: ViteJS memungkinkan pengguna untuk memilih paket npm yang ingin mereka gunakan di aplikasi mereka secara fleksibel. Ini membantu mengurangi beban aplikasi dengan hanya memasukkan paket yang benar-benar dibutuhkan.

## Konfigurasi
Buka file `vite.config.js` lalu ubah menjadi seperti kode berikut:

```php
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
	plugins: [
		laravel({
			input: [
				'resources/css/app.css',
				'resources/js/app.js'
			],
			refresh: [
				'app/**',
				'routes/**',
				'resources/views/**',
			],
		}),
	],
});
```

Dibagian `refresh` saya menempatkan direktori `app/**` supaya ketika ada perubahan di Backend bisa di refresh secara otomatis.

## Npm
Jika belum install npm, silahkan ketik perintah `npm install` lalu `npm run dev` Terakhir sematkan `@vite('resources/js/app.js')` di bagian `<head>`

Referensi :[https://laravel-news.com/laravel-blade-hot-refresh-with-vite](https://laravel-news.com/laravel-blade-hot-refresh-with-vite)

Date : 16-03-2024