#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Install NPM|Install NPM]]
- [[#Webpack|Webpack]]

## Introduction
Melakukan perubahan halaman dengan menekan tombol f5(refresh) sangat melelahkan. Laravel menyediakan Laravel mix yg dapat digunakan salah satunya untuk _auto refresh_ setelah perubahan.

## Install NPM
kenapa harus npm? karena Laravel Mix menggunakan javascript. NPM digunakan untuk menginstall library atau plugin yg tulis pada Javascript.

```
npm install
```

## Webpack
pada webpack.mix.js ketik kode berikut dipaling bawah!

```
mix.browserSync('127.0.0.1:8000');
```

```
npm run dev (otomatis terinstall browserSync)
npm run watch
```

Note : kita akan diarahkan ke port localhost:3000 bila menggunakan browserSync, namun jangan lupa aktifkan server larave