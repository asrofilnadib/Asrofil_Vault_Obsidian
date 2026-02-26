#Tech 
## Introduction
Kendala ketika update program di CPANEL kan gue compress dulu programnya dari local lalu upload di CPANEL. Itu buat kita kerepotan kalau update nya sangat sering. Sebelumnya gue sudah berhasil [[Menggunakan FTP Client Dari CPANEL]], Kali ini gue mau nyambungin FTP di VSCode. Ketika kita ada pembaharuan otomatis di CPANEL diperbarui.

## Install FTP Account di CPANEL
lihat Jurnal -> [[Menggunakan FTP Client Dari CPANEL]]

## FTP-Simple Ekstensi
![[ftp_simple_vscode_1.png]]
- Install Ekstensi Ftp-Simple terlebih dahulu di VSCode.

## Konfigurasi FTP

![[ftp_simple_vscode_2.png]]

- Ketik `ctrl + shift + p` lalu ketik ftp-simple. Pilih yg `Config - FTP Connection Setting`

![[ftp_simple_vscode_3.png]]

- Silahkan sesuaikan dengan informasi FTP lo. 

## Goto Folder
![[ftp_simple_vscode_4.png]]

- Pilih `Remote directory open to workspace`

![[ftp_simple_vscode_5.png]]

- Pilih nama FTP Server yg tadi udah lo buat.

![[ftp_simple_vscode_6.png]]

- Pilih folder yg ingin dituju.

## Folder Tidak Terbaca?
![[ftp_simple_vscode_9.png]]

- Lo cukup klik aja file nya maka akan muncul seperti gambar dibawah:

![[ftp_simple_vscode_10.png]]
## Download terlalu lama?
![[ftp_simple_vscode_1_2.png]]

Gue pernah ketika masuk FTP, download  nya lama. Jadi ternyata kita bisa set supaya download nya tidak lama. Ketik `ctrl + ,` lalu ketik `ftp-simple:remote-workspace-load-all` ubah dari true menjadi false.

![[ftp_simple_vscode_8.png]]

Date: 06-05-2025
