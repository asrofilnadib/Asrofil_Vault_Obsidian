#Tech #Tmux #Bash #Ubuntu #DevEnvironment #Automation

# Table of contents
- [[#Introduction]]
- [[#Tmux Layout Overview]]
- [[#Permanent Layout: main Session]]
- [[#Special Layout: tmux-catastrophy.sh]]
- [[#Bashrc Integration]]
- [[#How to Use]]
- [[#Creating Your Own Layout Script]]
- [[#Troubleshooting]]

## Introduction

Anda telah mengonfigurasi **tmux** untuk otomatis memulai sesi dengan **layout multi-pane yang kompleks** setiap kali membuka terminal, melalui integrasi di file `~/.bashrc`. Selain itu, Anda juga memiliki **layout khusus bernama “catastrophy”** yang bisa dijalankan via alias.

Catatan ini menjelaskan struktur layout tersebut, cara kerjanya, dan bagaimana Anda bisa memodifikasi atau memperluasnya.

---

## Tmux Layout Overview

Dua konfigurasi utama:

1. **Default Layout (`main`)**  
   → Diaktifkan **otomatis saat membuka terminal baru** (jika tidak dalam SSH dan tidak ada sesi tmux aktif).  
   → Terdiri dari **4 pane** dengan aplikasi bawaan: `btop`, `neofetch`, dan dua pane kosong untuk kerja.

2. **Special Layout (`catastrophy`)**  
   → Diaktifkan secara manual dengan perintah `catastrophy`.  
   → Menggunakan skrip terpisah: `~/.tmux-catastrophy.sh` (meskipun belum diunggah, alias-nya sudah ada).

---

## Permanent Layout: main Session

Berikut struktur layout yang dibuat oleh blok kode di `.bashrc`:

```
+---------------------+---------------------------+
|                     |        Pane 0.1           |
|      Pane 0.0       +-------------+-------------+
|      (btop)         | Pane 0.3    |   Pane 0.1  |
|                     | (clear)     |   (active)  |
+----------+----------+-------------+-------------+
|          |                                         |
|          |            Pane 0.2 (neofetch)          |
|          |                                         |
+----------+-----------------------------------------+
```

### Detail Pane:
- **Pane 0.0** (kiri): Lebar 80 kolom → menjalankan `btop` (monitoring sistem).
- **Pane 0.1** (kanan atas kanan): Fokus awal → siap untuk perintah.
- **Pane 0.3** (kanan atas kiri): Bersih (`clear`).
- **Pane 0.2** (kanan bawah): Menjalankan `neofetch` (info sistem).

### Fitur:
- Jika sesi `main` sudah ada → langsung **attach**, bukan buat baru.
- Menggunakan **pane numbering eksplisit** (`0.0`, `0.1`, dll) untuk kontrol presisi.
- Layout bertahan meskipun terminal ditutup — cukup buka terminal baru untuk kembali.

---

## Special Layout: tmux-catastrophy.sh

Di `.bashrc`, Anda memiliki alias:

```bash
alias catastrophy="bash ~/.tmux-catastrophy.sh"
```

Meskipun file `~/.tmux-catastrophy.sh` belum tersedia di unggahan, **tujuannya kemungkinan besar** adalah membuat sesi tmux dengan layout berbeda — misalnya untuk demo, pemantauan khusus, atau pengujian.

> 💡 Jika Anda ingin membuatnya, lihat contoh di bagian [[#Creating Your Own Layout Script]].

---

## Bashrc Integration

Bagian relevan di `~/.bashrc`:

```bash
if command -v tmux &> /dev/null && [ -z "$TMUX" ] && [ -z "$SSH_TTY" ]; then
    if tmux has-session -t main 2>/dev/null; then
        tmux attach-session -t main
    else
        # ... (buat sesi main dengan layout 4-pane)
    fi
fi
```

### Logika:
- Hanya berjalan di **local terminal** (bukan SSH).
- Tidak berjalan jika sudah dalam sesi tmux (`$TMUX` tidak kosong).
- Mencegah duplikasi sesi dengan memeriksa `tmux has-session`.

---

## How to Use

### 1. Buka terminal → layout `main` otomatis muncul.
- Fokus langsung di **pane kerja (0.1)**.
- `btop` dan `neofetch` sudah berjalan di latar.

### 2. Jalankan layout khusus (jika `~/.tmux-catastrophy.sh` ada):
```bash
catastrophy
```

### 3. Reload layout tanpa restart terminal:
- Tekan prefix (`Ctrl+a`), lalu tekan `r` → reload konfigurasi tmux.
- Untuk layout `main`, cukup **keluar dari sesi tmux** (`Ctrl+a` → `d`), lalu buka terminal baru.

---

## Creating Your Own Layout Script

Buat file `~/.tmux-catastrophy.sh`:

```bash
#!/bin/bash
SESSION="catastrophy"

# Hancurkan sesi lama jika ada
tmux kill-session -t $SESSION 2>/dev/null

# Buat sesi baru tersembunyi
tmux new-session -d -s $SESSION

# Split layout sesuai keinginan
tmux split-window -h -t $SESSION
tmux split-window -v -t $SESSION:0.1
tmux split-window -v -t $SESSION:0.0

# Kirim perintah ke masing-masing pane
tmux send-keys -t $SESSION:0.0 'htop' C-m
tmux send-keys -t $SESSION:0.1 'watch -n 1 date' C-m
tmux send-keys -t $SESSION:0.2 'tail -f /var/log/syslog' C-m
tmux send-keys -t $SESSION:0.3 'echo "CATASTROPHIC MODE ACTIVE"' C-m

# Attach ke sesi
tmux attach -t $SESSION
```

Beri izin eksekusi:
```bash
chmod +x ~/.tmux-catastrophy.sh
```

Sekarang, perintah `catastrophy` akan membuka sesi khusus ini.

---

## Troubleshooting

### ❌ “Tmux tidak otomatis jalan di terminal”
- Pastikan Anda **tidak dalam sesi SSH** (karena ada pengecekan `[ -z "$SSH_TTY" ]`).
- Pastikan `tmux` terinstal: `sudo apt install tmux`.
- Jika Anda login via **Wayland**, beberapa terminal (seperti GNOME Terminal) mungkin memperlakukan sesi berbeda — coba di **Alacritty** atau **Kitty**.

### 🔁 “Pane tidak sesuai ukuran”
- Pastikan terminal Anda cukup lebar (≥ 160 kolom) dan tinggi (≥ 40 baris).
- Resize terminal setelah sesi dimulai → tmux akan menyesuaikan.

### 🧯 “Mau reset semua sesi?”
```bash
tmux kill-server
```
Lalu buka terminal baru — sesi `main` akan dibuat ulang dari awal.

---

Dengan setup ini, Anda memiliki **lingkungan terminal yang konsisten, informatif, dan siap produktif** setiap kali membuka terminal — tanpa perlu konfigurasi manual berulang.