#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Solusinya|Solusinya]]

## Introduction
Ketika saya mendapatkan project untuk orang engineering, ada case dimana orang teknisi ini tidak sengaja melakukan swipe refresh seperti ini:

<video width="250" controls>
    <source src="WhatsApp Video 2024-12-30 at 14.45.32_9c3564a6.mp4" type="video/mp4">
</video>

Nah saya ingin seperti ini:

<video width="250" controls>
    <source src="WhatsApp Video 2024-12-30 at 14.45.33_e6d40d8b.mp4" type="video/mp4">
</video>
Ketika tidak sengaja melakukan refresh ini akan merugikan user karena mereka harus mencari jaringan yg stabil untuk menggunakan aplikasi tersebut.

**Kok nyari jaringan yg stabil?**

Karena saya pakai IndexedDB. Disana sangat buruk keadaan jaringan nya.

## Solusinya
```css
html, body {
    overscroll-behavior: none;
}
```

Mudah bukan ?

Date: 30-12-2024