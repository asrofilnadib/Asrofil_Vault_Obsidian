![[Pasted image 20250414090329.png]]
#Tech #Linux 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Ubah Kepemilikan|Ubah Kepemilikan]]
- [[#Hasil|Hasil]]

## Introduction
Error ini terkait dengan **SUID sandbox** adalah masalah umum pada aplikasi berbasis Electron (seperti DB Gate) di Linux. Masalah ini muncul karena file `chrome-sandbox` tidak memiliki izin yang benar atau tidak dimiliki oleh pengguna `root`.

## Ubah Kepemilikan
Cukup ketik perintah dibawah:

```bash
cd /opt/DbGate/
sudo chown root:root /opt/DbGate/chrome-sandbox
sudo chmod 4755 /opt/DbGate/chrome-sandbox
```

## Hasil
![[Pasted image 20250414090559.png]]

Date: 14-04-2025