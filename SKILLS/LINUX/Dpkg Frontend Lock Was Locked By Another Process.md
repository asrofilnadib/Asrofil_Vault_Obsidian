![[Screenshot from 2025-04-13 06-17-23.png]]
#Tech #Linux
# Table of Content
- [[#Introduction|Introduction]]
- [[#Cara Mengatasinya|Cara Mengatasinya]]

## Introduction
Ketika gue sedang install salah satu software di Linux, sering kali mengalami error seperti ini. Error "dpkg frontend locked by another process" terjadi ketika ada proses lain yang sedang menggunakan dpkg, seperti saat menginstal atau memperbarui perangkat lunak. Ini biasanya disebabkan oleh proses yang berjalan di latar belakang, seperti pembaruan otomatis atau instalasi yang belum selesai.

## Cara Mengatasinya
Cukup mudah dengan mengetik perintah dibawah:

```bash
sudo kill -9 7891
```

PID tersebut harus di kill (terminate) prosesnya agar kita bisa install yg lain.
Note: -9 = sembilan bukan G.

Date: 13-04-2025
