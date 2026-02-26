#Tech #Tmux #Clipboard #Ubuntu

# Table of contents
- [[#Introduction]]
- [[#How It Works]]
- [[#Enable Mouse Copy-Paste in Tmux]]
- [[#Reload Tmux Configuration]]
- [[#Additional Notes]]

## Introduction

Tmux secara default tidak mengaktifkan fitur "highlight-to-copy" seperti yang biasa kita alami di terminal biasa. Namun, dengan sedikit konfigurasi, Anda bisa membuat tmux langsung menyalin teks ke clipboard sistem hanya dengan menyorot (highlight) teks menggunakan mouse — tanpa perlu menekan shortcut tambahan atau masuk ke mode copy manual.

## How It Works

Fitur ini mengandalkan:
1. **Mouse mode** di tmux (`set -g mouse on`) untuk mengaktifkan interaksi mouse.
2. **Integrasi dengan clipboard sistem** melalui `xclip` (X11) atau `wl-copy` (Wayland).

Di banyak terminal modern, menyorot teks otomatis menyalin ke **PRIMARY selection** X11 (tempel dengan klik tengah), tetapi **tidak ke CLIPBOARD** (yang digunakan oleh Ctrl+V). Konfigurasi berikut memastikan teks disalin ke **CLIPBOARD**, sehingga bisa ditempel di mana saja dengan `Ctrl+V`.

## Enable Mouse Copy-Paste in Tmux

Tambahkan baris berikut ke file konfigurasi tmux Anda: `~/.tmux.conf`.

```bash
# Aktifkan mouse untuk scroll, resize pane, dan seleksi teks
set -g mouse on

# Salin teks yang disorot langsung ke clipboard sistem
# Gunakan wl-copy jika Anda menggunakan Wayland (seperti Ubuntu 24.04)
bind -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel 'wl-copy'

# Jika Anda menggunakan X11 (bukan Wayland), ganti dengan:
# bind -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'
```

> 💡 **Pastikan dependensi terinstal**:

```bash
# Untuk Wayland (default di Ubuntu 24.04)
sudo apt install wl-clipboard
```

```bash
# Untuk X11 (jika diperlukan)
sudo apt install xclip
```

Setelah konfigurasi ini:
- Saat Anda **menyorot teks sambil menahan tombol kiri mouse**, tmux akan:
  - Menyalin teks tersebut ke clipboard sistem.
  - Keluar otomatis dari mode copy (`copy-pipe-and-cancel`).
- Anda bisa langsung **tempel di mana saja dengan `Ctrl+V`**.

## Reload Tmux Configuration

Setelah menyimpan perubahan di `~/.tmux.conf`, reload konfigurasi tanpa perlu keluar dari sesi tmux:

```bash
tmux source-file ~/.tmux.conf
```

Atau, jika Anda sedang berada di dalam sesi tmux:
1. Tekan **prefix key** (biasanya `Ctrl+b`).
2. Ketik perintah berikut lalu tekan Enter:
   ```
   :source-file ~/.tmux.conf
   ```

## Additional Notes

- **Mode keyboard alternatif**: Jika tidak ingin pakai mouse, gunakan mode vi:
  - Tekan `Ctrl+b` lalu `[` untuk masuk copy mode.
  - Navigasi dengan `h`, `j`, `k`, `l`.
  - Tekan `Space` untuk mulai seleksi, `Enter` untuk menyalin.
- **Wayland & Ubuntu 24.04**: Karena Anda menggunakan Wayland, `wl-copy` adalah pilihan yang tepat. Pastikan paket `wl-clipboard` terinstal.
- **Terminal modern**: Beberapa terminal seperti **Kitty** atau **Alacritty** memiliki integrasi clipboard bawaan yang mungkin sudah bekerja tanpa konfigurasi tmux. Namun, konfigurasi di atas memastikan konsistensi di semua lingkungan.
- **Versi tmux**: Fitur `copy-pipe-and-cancel` dan `MouseDragEnd1Pane` tersedia mulai tmux versi **2.4+**. Disarankan menggunakan tmux ≥ **3.0**. Cek versi Anda dengan:
  ```bash
  tmux -V
  ```

Dengan konfigurasi ini, workflow copy-paste di tmux menjadi sangat lancar: **sorot → lepas → tempel** — tanpa shortcut tambahan.