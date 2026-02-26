t#Tech #Devilspie2 #WindowManagement #Ubuntu #Automation

# Table of contents
- [[#Introduction]]
- [[#Install Devilspie2]]
- [[#Configuration Directory]]
- [[#rules.lua Script Explanation]]
- [[#Full rules.lua Content]]
- [[#Autostart Devilspie2 on Login]]
- [[#Troubleshooting Tips]]
- [[#Useful Commands]]

## Introduction

Devilspie2 adalah utilitas window-matching untuk X11 yang memungkinkan Anda **mengotomatiskan posisi, ukuran, workspace, dan properti jendela** berdasarkan kelas aplikasi, nama jendela, atau kriteria lainnya. Cocok untuk membangun lingkungan pengembangan yang konsisten dan produktif — terutama di Ubuntu dengan GNOME (atau DE lain berbasis X11).

Catatan ini menjelaskan cara mengonfigurasi **Devilspie2** agar jendela aplikasi seperti **Brave, Obsidian, VS Code, WhatsApp, Telegram, Discord, dan Spotify** langsung muncul di **workspace dan posisi yang telah ditentukan**.

> ⚠️ **Catatan Penting**: Devilspie2 hanya berjalan di **X11**, bukan Wayland. Jika Anda menggunakan **Wayland** (default di Ubuntu 24.04), Anda perlu [login sebagai sesi X11](#troubleshooting-tips) agar Devilspie2 berfungsi.

## Install Devilspie2

Instal paket dari repositori resmi Ubuntu:

```bash
sudo apt update
sudo apt install devilspie2
```

## Configuration Directory

Buat direktori konfigurasi di home Anda:

```bash
mkdir -p ~/.config/devilspie2
```

File skrip utama harus bernama **`rules.lua`** dan ditempatkan di:
```
~/.config/devilspie2/rules.lua
```

## rules.lua Script Explanation

Skrip Lua berikut menggunakan fungsi helper `move_window()` untuk:
- Memindahkan jendela ke **workspace tertentu**
- Menyetel **posisi (x, y)** dan **ukuran (lebar, tinggi)**

Setiap blok `if` memeriksa **window class** (bisa dicek dengan `xprop`) dan menerapkan aturan jika cocok.

### Workspace Mapping
- **Workspace 1**: Brave Browser
- **Workspace 2**: Obsidian & VS Code
- **Workspace 4**: WhatsApp (kiri), Telegram (kanan), Discord (full)
- **Workspace 5**: Spotify (full screen)

### Resolusi Asumsi
- Layar utama: **1920×1080**
- Dua jendela side-by-side: masing-masing **960×1080**

## Full rules.lua Content

Simpan isi berikut ke `~/.config/devilspie2/rules.lua`:

```lua
-- Helper function to move and resize window
function move_window(ws, x, y, w, h)
    if ws ~= nil then
        set_window_workspace(ws)
    end
    if x ~= nil and y ~= nil then
        set_window_position(x, y)
    end
    if w ~= nil and h ~= nil then
        set_window_size(w, h)
    end
end

-- Brave Browser
if (get_window_class() == "Brave-browser") then
    move_window(1, 960, 590, 1920, 1080)
end

-- Obsidian
if (get_window_class() == "obsidian") then
    move_window(2, 960, 590, 1920, 1080)
end

-- VSCode
if (get_window_class() == "Code") then
    move_window(2, 960, 590, 1920, 1080)
end

-- Whatsapp (ZapZap Desktop)
if (get_window_class() == "ZapZap") then
    move_window(4, 0, 0, 960, 1080)
end

-- Telegram
if (get_window_class() == "TelegramDesktop") then
    move_window(4, 960, 0, 960, 1080)
end

-- Discord
if (get_window_class() == "discord") then
    move_window(4, 0, 0, 1920, 1080)
end

-- Spotify
if (get_window_class() == "Spotify") then
    move_window(5, 0, 0, 1920, 1080)
end
```

> 💡 **Catatan**:
> - `get_window_class()` mengembalikan **WM_CLASS** dari jendela.
> - Untuk memeriksa class aplikasi Anda, jalankan:

```bash
xprop | grep WM_CLASS
```

>   Lalu klik jendela yang ingin Anda identifikasi.

## Autostart Devilspie2 on Login

Agar Devilspie2 otomatis berjalan saat login:

1. Buka **Startup Applications** (cari di GNOME Activities).
2. Klik **Add**.
3. Isi:
   - **Name**: `Devilspie2`
   - **Command**: `devilspie2`
   - **Comment**: Optional

Atau buat file `.desktop` secara manual:

```bash
mkdir -p ~/.config/autostart
cat > ~/.config/autostart/devilspie2.desktop 

<<EOF
[Desktop Entry]
Type=Application
Exec=devilspie2
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=Devilspie2
Comment=Automatic window rules
EOF
```

## Troubleshooting Tips

### ❌ "Devilspie2 doesn't work on Ubuntu 24.04"
- Devilspie2 **tidak kompatibel dengan Wayland**.
- Saat login, pilih **"Ubuntu on Xorg"** (bukan "Ubuntu") di layar login GNOME.
  - Klik ikon roda gigi (⚙️) di pojok kanan bawah sebelum login.

### ❓ "Bagaimana tahu window class-nya?"
Jalankan:
```bash
xprop | grep "WM_CLASS"
```
Lalu klik jendela target. Output contoh:
```
WM_CLASS(STRING) = "code", "Code"
```
Gunakan nilai **kedua** (`"Code"`), itulah yang digunakan oleh `get_window_class()`.

### 🔁 "Aturan tidak diterapkan saat membuka aplikasi"
- Pastikan Devilspie2 sudah berjalan **sebelum** membuka aplikasi.
- Devilspie2 hanya menangkap jendela saat **dibuat**, bukan saat difokuskan ulang.

## Useful Commands

| Command | Description |
|--------|-------------|
| `devilspie2 --debug` | Jalankan dengan logging untuk debugging |
| `pkill devilspie2 && devilspie2` | Restart Devilspie2 setelah ubah `rules.lua` |
| `xdotool search --class obsidian get_desktop` | Cek workspace jendela (untuk verifikasi) |

Dengan Devilspie2, Anda bisa membangun **workspace layout yang konsisten dan otomatis** — tanpa perlu menyeret atau mengatur jendela manual setiap kali membuka aplikasi.