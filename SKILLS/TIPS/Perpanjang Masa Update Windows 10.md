#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
Windows kan default nya update cuma beberapa bulan kedepan ya. Ini bisa 2 tahun kedepan. Cara nya? liat gambar dibawah

![[ExtendWinPause(1).png]]

Ke registry Editor, ke **HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > WindowsUpdate > UX > Settings**

Buat file dengan nama **FlightSettingsMaxPauseDays** dengan klik kanan, pilih new, pilih DWORD 32 bit value.

<img src="ExtendWinPause(2).png" width="350" align="left">








Setelah itu klik file, pilih desimal. masukkan 1000. Terakhir ke update windows pilih advance options

Work at windows 10 Pro

Date: 18-12-2024