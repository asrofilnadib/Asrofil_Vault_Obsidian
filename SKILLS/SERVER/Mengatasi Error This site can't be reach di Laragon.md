![[Cannot be reach.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Lets solved|Lets solved]]
- [[#Install SSL|Install SSL]]

## Introduction
Ketika saya buat project baru, biasanya laragon akan otomatis membuatkan virtual host untuk kita. _Virtual host (atau virtual hosting)_ adalah praktik dalam teknologi web di mana satu _server fisik_ dapat meng-_host beberapa domain_ atau situs web secara _bersamaan_. Ada kalanya laragon secara tidak otomatis membuatkan virtual host untuk kita. Kita harus membuat manual.

## Lets solved
1. Silahkan buka file pada `C:\windows\System32\drivers\etc\hosts`
2. Ubah seperti gambar dibawah, lalu klik retry as admin

![[Laragon error cant be reached_1.png]]

3. Jika sebelumnya terdapat error readonly atau tidak bisa di save maka matikan readonly file hosts nya dengan klik kanan, pilih properties

<img src="Laragon error cant be reached_2.png" width="350" align="left">












4. Lalu un-check read-only nya

<img src="Laragon error cant be reached_3.png" width="250" align="left">

## Install SSL
<img src="Laragon error cant be reached_4.png" width="450" align="left">













Untuk install ssl, silahkan klik setting dan pergi ke services & ports dan Enable ssl nya!

NOTE : Lebih prefer menggunakan `.test` atau `.local` agar bila install ssl tidak dianggap ini domain. **Konflik dengan TLD (Top Level Domain)**: Beberapa program atau server lokal mungkin menggunakan ekstensi TLD tertentu (seperti `.dev`) untuk keperluan internal atau penggunaan khusus. Ini bisa menyebabkan konflik ketika Anda mencoba menggunakan domain dengan TLD yang sama di Laragon. -**Chatgpt**

Date : 02-07-2024