#Tech #GNOME #Ubuntu #DesktopEnvironment #Extensions

# Table of contents
- [[#Introduction]]
- [[#Install GNOME Desktop (if not default)]]
- [[#Switch to GNOME Session at Login]]
- [[#Install GNOME Extensions Manager]]
- [[#Recommended GNOME Extensions]]
- [[#How to Install and Manage Extensions]]
- [[#Troubleshooting Tips]]

## Introduction

Ubuntu 24.04 LTS menggunakan **GNOME** sebagai desktop environment default. Namun, jika Anda menginstal versi minimal atau server, Anda mungkin perlu menginstal GNOME secara manual. Selain itu, untuk pengalaman yang lebih produktif dan personalisasi, **GNOME Extensions** adalah kunci utama.

Catatan ini akan membantu Anda:
1. Menginstal GNOME Desktop jika belum ada.
2. Mengaktifkan sesi GNOME saat login.
3. Menginstal dan mengelola ekstensi GNOME yang populer dan berguna.

---

## Install GNOME Desktop (if not default)

Jika Anda menggunakan **Ubuntu Server** atau instalasi minimal, jalankan:

```bash
sudo apt update
sudo apt install ubuntu-desktop
```

> 💡 Jika Anda ingin versi GNOME yang lebih ringan (tanpa aplikasi tambahan), gunakan:

```bash
sudo apt install gnome-session
```

Setelah instalasi selesai, reboot sistem:

```bash
sudo reboot
```

---

## Switch to GNOME Session at Login

Saat login ke Ubuntu, di layar login (GDM), klik ikon roda gigi (⚙️) di pojok kanan bawah, lalu pilih:

> **Ubuntu** → untuk sesi GNOME dengan Wayland (default)  
> **Ubuntu on Xorg** → jika Anda perlu kompatibilitas dengan aplikasi lama (misalnya Devilspie2)

---

## Install GNOME Extensions Manager

Untuk mengelola ekstensi dengan mudah, instal **GNOME Extensions** dari Snap Store atau via terminal:

```bash
sudo snap install gnome-extensions
```

Atau, jika Anda lebih suka versi GUI, buka **Ubuntu Software**, cari “**GNOME Extensions**”, dan instal.

---

## Recommended GNOME Extensions

Berdasarkan gambar yang Anda berikan, berikut daftar ekstensi yang sudah Anda gunakan — ditambah rekomendasi lain yang sangat berguna untuk produktivitas dan estetika:

### ✅ User-Installed Extensions (from your screenshot)

| Extension Name | Purpose |
|----------------|---------|
| **App Icons Taskbar** | Menampilkan ikon aplikasi di taskbar (dock). |
| **AppIndicator and KStatusNotifierItem Support** | Mendukung ikon status tray dari aplikasi seperti Discord, Telegram, Spotify. |
| **ArcMenu** | Menu aplikasi modern ala Windows/macOS. |
| **Blur my Shell** | Efek blur pada panel, dock, dan jendela. |
| **Custom Accent Colors** | Mengganti warna aksen GNOME dengan warna pilihan Anda. |
| **Dash to Dock** | Mengubah dash (launcher) menjadi dock di sisi layar. |
| **Forge** | Manajer tema dan ikon yang powerful. |
| **GNOME Fuzzy App Search** | Pencarian aplikasi dengan fuzzy matching (lebih cepat dan akurat). |
| **GSConnect** | Integrasi dengan Android (mirip KDE Connect). |
| **User Themes** | Memungkinkan penggunaan tema dari file `.zip` atau `gnome-look.org`. |
| **Vitals** | Menampilkan informasi sistem real-time: CPU, RAM, disk, suhu, dll. |

### 🆕 Additional Highly Recommended Extensions

| Extension Name | Purpose |
|----------------|---------|
| **Just Perfection** | Menyesuaikan hampir semua aspek GNOME: animasi, panel, tombol, dll. |
| **OpenWeather** | Widget cuaca di panel atas. |
| **Clipboard Indicator** | Riwayat clipboard di panel. |
| **Caffeine** | Mencegah layar mati saat presentasi atau menonton video. |
| **Sound Input & Output Device Chooser** | Mudah ganti input/output audio dari panel. |
| **TopIcons Plus** (jika Anda butuh legacy tray icons) | Alternatif untuk AppIndicator. |

---

## How to Install and Manage Extensions

### Method 1: Via GNOME Extensions Website (Easiest)

1. Buka [https://extensions.gnome.org](https://extensions.gnome.org)
2. Klik tombol **“Install browser extension”** (untuk Firefox/Chrome).
3. Setelah terinstal, Anda bisa mengaktifkan ekstensi langsung dari situs web.
4. Atau, buka aplikasi **GNOME Extensions** di menu aplikasi untuk mengelola.

### Method 2: Via Terminal (Advanced)

Contoh: Instal ekstensi “Dash to Dock”

```bash
# Download extension
wget https://extensions.gnome.org/download-extension/dash-to-dock@micxgx.gmail.com.shell-extension.zip

# Extract to ~/.local/share/gnome-shell/extensions/
mkdir -p ~/.local/share/gnome-shell/extensions/
unzip dash-to-dock@micxgx.gmail.com.shell-extension.zip -d ~/.local/share/gnome-shell/extensions/dash-to-dock@micxgx.gmail.com/

# Restart GNOME Shell (tekan Alt+F2, lalu ketik "r" + Enter)
```

> ⚠️ Pastikan Anda telah menginstal `chrome-gnome-shell` atau `firefox-gnome-extensions` untuk integrasi browser.

---

## Troubleshooting Tips

### ❌ “Extension not showing in GNOME Extensions app”
- Pastikan Anda menjalankan **GNOME session**, bukan Wayland. Coba login dengan **“Ubuntu on Xorg”**.
- Restart GNOME Shell: tekan `Alt+F2`, ketik `r`, lalu Enter.

### ❓ “Bagaimana cara menghapus ekstensi?”
- Buka aplikasi **GNOME Extensions** → klik tombol gear → pilih **“Remove”**.
- Atau hapus folder ekstensi dari:
  ```bash
  rm -rf ~/.local/share/gnome-shell/extensions/extension-name@domain.com
  ```

### 🔁 “Tidak bisa menginstal ekstensi dari website”
- Instal paket `chrome-gnome-shell`:
  ```bash
  sudo apt install chrome-gnome-shell
  ```
- Untuk Firefox, pasang add-on “GNOME Integration”.

Dengan konfigurasi GNOME dan ekstensi yang tepat, Anda bisa menciptakan desktop yang **sangat personal, efisien, dan indah** — cocok untuk developer, desainer, atau siapa saja yang menginginkan kontrol penuh atas lingkungan kerja mereka.