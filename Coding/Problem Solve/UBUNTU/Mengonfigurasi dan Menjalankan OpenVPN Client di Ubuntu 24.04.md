#Tech #OpenVPN #Networking #Ubuntu #VPN #Security

# Table of contents
- [[#Introduction]]
- [[#Prasyarat]]
- [[#Langkah 1: Pindahkan Konfigurasi ke Direktori OpenVPN]]
- [[#Langkah 2: Aktifkan dan Jalankan Layanan OpenVPN Client]]
- [[#Langkah 3: Verifikasi Koneksi]]
- [[#Catatan Penting: Autentikasi Username/Password]]
- [[#Troubleshooting Umum]]
- [[#Referensi Perintah Cepat]]

## Introduction

Anda memiliki file konfigurasi OpenVPN bernama `client.ovpn` yang tersimpan di:
```
~/Documents/PAS/client.ovpn
```

File ini digunakan untuk menyambungkan ke server OpenVPN eksternal (misalnya: kantor, cloud, atau layanan pribadi). Catatan ini menjelaskan cara mengintegrasikan file tersebut dengan **layanan systemd OpenVPN client** di Ubuntu 24.04 agar bisa dijalankan secara stabil dan otomatis.

---

## Prasyarat

- Sistem: **Ubuntu 24.04** (atau distro berbasis Debian)
- File: `client.ovpn` sudah tersedia
- Anda memiliki **akses sudo**
- OpenVPN sudah terinstal:
  ```bash
  sudo apt install openvpn
  ```

> ❌ Jangan gunakan `snap` untuk OpenVPN client — lebih baik gunakan versi dari repositori resmi (`apt`).

---

## Langkah 1: Pindahkan Konfigurasi ke Direktori OpenVPN

File konfigurasi client harus ditempatkan di:
```
/etc/openvpn/client/
```

Ganti ekstensi `.ovpn` menjadi `.conf`:

```bash
sudo cp ~/Documents/PAS/client.ovpn /etc/openvpn/client/client.conf
```

> ⚠️ **Penting**:  
> - Nama file tanpa `.ovpn` → cukup `client.conf`  
> - Pastikan file bisa dibaca oleh root:
```bash
sudo chmod 644 /etc/openvpn/client/client.conf
```

---

## Langkah 2: Aktifkan dan Jalankan Layanan OpenVPN Client

### Jalankan sementara (untuk uji coba):

```bash
sudo systemctl start openvpn-client@client
```

> 🔐 Sistem akan meminta:
> - **Username autentikasi**
> - **Password autentikasi**  
> (jika file `client.ovpn` mengandung `auth-user-pass`)

> 💡 Jika diminta via “broadcast message”, tetap masukkan di terminal Anda — systemd akan meneruskannya ke OpenVPN.

### Aktifkan otomatis saat boot:

```bash
sudo systemctl enable openvpn-client@client
```

---

## Langkah 3: Verifikasi Koneksi

### Cek status layanan:

```bash
sudo systemctl status openvpn-client@client
```

Jika sukses, Anda akan melihat:
```
Active: active (running)
Status: "Initialization Sequence Completed"
```

### Cek IP publik (pastikan berubah):

```bash
curl ifconfig.me
```

Jika IP berbeda dari sebelumnya → koneksi berhasil.

### Cek antarmuka tun:

```bash
ip a show dev tun0
```

Anda harus melihat antarmuka `tun0` dengan alamat IP dari jaringan VPN.

---

## Catatan Penting: Autentikasi Username/Password

Jika file `client.ovpn` berisi baris:

```
auth-user-pass
```

…maka OpenVPN akan meminta username & password setiap kali dijalankan via systemd.

### Solusi untuk otomatisasi (opsional):

Buat file kredensial terpisah:

```bash
sudo nano /etc/openvpn/client/auth.txt
```

Isi dengan:
```
username_anda
password_anda
```

Lalu ubah `client.conf`:
```diff
- auth-user-pass
+ auth-user-pass auth.txt
```

Dan atur izin:
```bash
sudo chmod 600 /etc/openvpn/client/auth.txt
sudo chown root:root /etc/openvpn/client/auth.txt
```

> 🔒 Jangan simpan password di file jika sistem multi-user!

---

## Troubleshooting Umum

### ❌ “Job for openvpn-client@client.service failed”

Periksa log detail:
```bash
sudo journalctl -u openvpn-client@client.service --since today
```

Umumnya disebabkan oleh:
- File konfigurasi salah nama (harus `.conf`, bukan `.ovpn`)
- Path tidak ditemukan
- Autentikasi gagal
- Port 1194/UDP diblokir firewall

### ❌ “No such file or directory” saat `cp`

Pastikan direktori `/etc/openvpn/client/` ada:
```bash
sudo mkdir -p /etc/openvpn/client
```

### ❌ Tidak bisa akses internet setelah koneksi

Cek DNS:
```bash
cat /etc/resolv.conf
```

Jika perlu, tambahkan di `client.conf`:
```
script-security 2
up /etc/openvpn/update-resolv-conf
down /etc/openvpn/update-resolv-conf
```

---

## Referensi Perintah Cepat

| Perintah | Deskripsi |
|--------|----------|
| `sudo cp ~/Documents/PAS/client.ovpn /etc/openvpn/client/client.conf` | Salin & rename konfig |
| `sudo systemctl start openvpn-client@client` | Mulai koneksi |
| `sudo systemctl enable openvpn-client@client` | Aktifkan auto-start |
| `sudo systemctl status openvpn-client@client` | Cek status |
| `curl ifconfig.me` | Verifikasi IP publik |
| `sudo journalctl -u openvpn-client@client -f` | Log real-time |

---

Dengan konfigurasi ini, koneksi OpenVPN Anda:
✅ Berjalan sebagai layanan systemd  
✅ Otomatis start saat boot  
✅ Terintegrasi dengan manajemen sistem Ubuntu  
✅ Aman dan mudah dikelola

Simpan note ini sebagai panduan permanen untuk deployment OpenVPN di lingkungan kerja Anda.