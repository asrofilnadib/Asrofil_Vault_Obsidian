#Tech #Superfile #Ubuntu #Tmux #Terminal #TUI

# Table of contents
- [[#Introduction]]
- [[#Install Superfile]]
- [[#Dependencies Opsional]]
- [[#Konfigurasi Dasar]]
- [[#Integrasi Tmux — Truecolor & COLORTERM]]
- [[#cd on Quit — Wrapper spf() di Bash]]
- [[#Font & Rendering]]
- [[#Rekomendasi config.toml]]
- [[#Verifikasi Setup]]
- [[#Troubleshooting]]
- [[#Referensi]]

## Introduction

[superfile](https://superfile.dev/) adalah terminal file manager modern (Go + Bubble Tea) dengan UI rapi, multi-panel, preview file, dan 20+ tema built-in. Catatan ini mencakup setup lengkap di **Ubuntu 24.04**, termasuk integrasi dengan **tmux** agar warna truecolor tampil benar.

Hotkey lengkap ada di → [[Superfile Hotkeys]]

---

## Install Superfile

### Metode 1: Install script (recommended)

```bash
bash -c "$(curl -sLo- https://superfile.dev/install.sh)"
```

Setelah selesai, jalankan:

```bash
spf
```

Binary biasanya terpasang di `~/.local/bin/spf`. Pastikan path ini ada di `PATH`:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

### Metode 2: Go install

```bash
go install github.com/yorukot/superfile@latest
```

### Path konfigurasi (Linux)

| Item | Path |
|------|------|
| Config utama | `~/.config/superfile/config.toml` |
| Hotkeys | `~/.config/superfile/hotkeys.toml` |
| Theme | `~/.config/superfile/theme/` |
| Data | `~/.local/share/superfile` |
| Log | `~/.local/state/superfile/superfile.log` |
| State (cd on quit) | `~/.local/state/superfile/lastdir` |

Edit config:

```bash
nano ~/.config/superfile/config.toml
```

---

## Dependencies Opsional

| Paket | Fungsi di superfile |
|-------|---------------------|
| **Nerd Font** | Icon file & folder (`nerdfont = true`) |
| `bat` | Syntax highlighting preview kode |
| `exiftool` | Metadata detail (`metadata = true`) |
| `zoxide` | Navigasi cepat via modal `z` |
| `unzip` / `p7zip` | Extract archive |
| `zip` | Compress ke `.zip` |

Install contoh:

```bash
sudo apt update
sudo apt install bat exiftool unzip zip
```

Install zoxide (opsional):

```bash
curl -sSfL https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | sh
```

---

## Konfigurasi Dasar

Pertama kali jalanin `spf`, config default otomatis dibuat. Beberapa setting penting:

```toml
# ~/.config/superfile/config.toml

theme = "catppuccin-mocha"        # ganti sesuai selera
default_directory = "."           # folder awal saat buka spf
cd_on_quit = true                 # wajib true kalau mau cd_quit (Q) & wrapper spf()
default_open_file_preview = true
show_image_preview = true
nerdfont = true                   # butuh Nerd Font di terminal
```

Tema tersedia: `catppuccin-mocha`, `tokyonight`, `dracula`, `gruvbox`, `nord`, dll. Lihat [Theme List](https://superfile.dev/list/theme-list/).

---

## Integrasi Tmux — Truecolor & COLORTERM

superfile (base Go/Bubble Tea) ngecek **`COLORTERM`**, bukan cuma `$TERM`. Di dalam tmux, `COLORTERM` sering **nggak ke-pass** ke pane child — ini sumber masalah warna hitam-putih / pucat paling sering.

### Setup minimal (yang paling penting)

**1. Export `COLORTERM` di shell** — tambahkan ke `~/.bashrc` (sebelum masuk tmux):

```bash
export COLORTERM=truecolor
```

**2. Pass `COLORTERM` ke pane tmux** — tambahkan ke `~/.tmux.conf`:

```bash
set -ga update-environment "COLORTERM"
```

> Ini yang sering kelewat. Tanpa baris ini, `COLORTERM` bisa kosong di dalam tmux meskipun udah di-export di `.bashrc`.

### Setup tambahan (recommended)

Kalau warna masih aneh setelah setup minimal di atas, tambahkan juga:

```bash
# Terminal default di dalam tmux
set -g default-terminal "tmux-256color"

# tmux 3.3+ — aktifkan RGB/truecolor
set -as terminal-features ",*:RGB"

# Fallback tmux lama (< 3.3), ganti XXX dengan $TERM di LUAR tmux:
# set -as terminal-overrides ",xterm-256color:Tc"
```

### Verifikasi

```bash
# Di LUAR tmux
echo $COLORTERM    # harus: truecolor

# Di DALAM tmux (buka pane baru setelah reload config)
echo $COLORTERM    # harus: truecolor (bukan kosong!)
echo $TERM         # biasanya: tmux-256color
```

### Reload tmux

```bash
tmux source-file ~/.tmux.conf
```

Lalu buka **sesi tmux baru** — attach ke sesi lama kadang masih pakai env lama.

### Ringkasan

| Setting | Di mana | Fungsi |
|---------|---------|--------|
| `export COLORTERM=truecolor` | `~/.bashrc` | Kasih tau app TUI bahwa terminal support 24-bit |
| `set -ga update-environment "COLORTERM"` | `~/.tmux.conf` | **Wajib** — tmux nerusin `COLORTERM` ke pane child |
| `default-terminal "tmux-256color"` | `~/.tmux.conf` | TERM yang benar di dalam tmux |
| `terminal-features ",*:RGB"` | `~/.tmux.conf` | tmux nerusin capability RGB ke app child |

---

## cd on Quit — Wrapper `spf()` di Bash

Agar `cd_on_quit` dan hotkey `Q` (cd_quit) bekerja, superfile butuh wrapper function di shell — bukan langsung panggil binary `spf`.

### 1. Aktifkan di config

```toml
cd_on_quit = true
```

### 2. Tambahkan wrapper ke `~/.bashrc`

```bash
spf() {
    os=$(uname -s)

    if [[ "$os" == "Linux" ]]; then
        export SPF_LAST_DIR="${XDG_STATE_HOME:-$HOME/.local/state}/superfile/lastdir"
    fi

    if [[ "$os" == "Darwin" ]]; then
        export SPF_LAST_DIR="$HOME/Library/Application Support/superfile/lastdir"
    fi

    command spf "$@"

    [ ! -f "$SPF_LAST_DIR" ] || {
        . "$SPF_LAST_DIR"
        rm -f -- "$SPF_LAST_DIR" > /dev/null
    }
}
```

### 3. Reload bashrc

```bash
source ~/.bashrc
```

Sekarang:
- Keluar normal → shell tetap di folder semula
- Tekan `Q` (shift+q) → shell `cd` ke folder terakhir di file panel

---

## Font & Rendering

### Nerd Font

Icon superfile butuh font yang sudah di-patch Nerd Font.

1. Download font (contoh: [JetBrainsMono Nerd Font](https://www.nerdfonts.com/font-downloads))
2. Install ke `~/.local/share/fonts/`
3. Set font tersebut di terminal emulator (GNOME Terminal, Kitty, Alacritty, dll)
4. Pastikan `nerdfont = true` di config

### Locale UTF-8

```bash
locale
```

Pastikan output mengandung `UTF-8`. Kalau belum:

```bash
sudo localectl set-locale LANG=en_US.UTF-8
# atau
sudo localectl set-locale LANG=id_ID.UTF-8
```

### Rendering rusak (karakter aneh / layout pecah)

Tambahkan ke `~/.bashrc`:

```bash
export RUNEWIDTH_EASTASIAN=0
```

---

## Rekomendasi config.toml

Contoh config yang nyaman dipakai sehari-hari di Ubuntu + tmux:

```toml
##############################################
#           Superfile Configuration          #
##############################################

editor = ""                        # pakai $EDITOR (nvim, code, dll)
cd_on_quit = true
auto_check_update = true

default_open_file_preview = true
show_image_preview = true
show_panel_footer_info = true
default_directory = "."
file_size_use_si = false

default_sort_type = 4              # Natural sort (file2 sebelum file10)
sort_order_reversed = false
case_sensitive_sort = false

theme = "catppuccin-mocha"
code_previewer = "bat"             # butuh bat terinstall
nerdfont = true
show_select_icons = true
transparent_background = false

file_preview_width = 2
enable_file_preview_border = true
sidebar_width = 20
sidebar_sections = ["home", "pinned", "disks"]

# Plugins
metadata = false                   # true kalau exiftool terinstall
zoxide_support = true              # true kalau zoxide terinstall

[open_with]
md = "nvim"
json = "nvim"
conf = "nvim"
```

Custom hotkey (vim user):

```bash
nano ~/.config/superfile/hotkeys.toml
```

Lihat [Custom Hotkeys](https://superfile.dev/configure/custom-hotkeys/).

---

## Verifikasi Setup

Jalankan checklist ini setelah setup:

```bash
# 1. Binary ada
which spf

# 2. COLORTERM ter-set (di luar DAN di dalam tmux)
echo $COLORTERM    # harus: truecolor atau 24bit

# 3. Di dalam tmux, cek lagi
tmux new-session -d -s spf-test 'echo "TERM=$TERM COLORTERM=$COLORTERM" > /tmp/tmux-env.txt'
cat /tmp/tmux-env.txt   # COLORTERM harus truecolor, TERM biasanya tmux-256color
tmux kill-session -t spf-test

# 4. Locale UTF-8
locale | grep UTF-8

# 5. Jalankan superfile
spf
```

Di dalam superfile:
- Tekan `?` → help menu muncul
- Tekan `f` → preview panel toggle
- Tekan `Q` → keluar + cd ke folder aktif (kalau wrapper sudah diset)

---

## Troubleshooting

### Icon tidak tampil / kotak kosong

- Install & apply **Nerd Font** di terminal
- Atau set `nerdfont = false` di config (tanpa icon)

### Warna pucat / hitam-putih di tmux

**Langkah 1 — cek `COLORTERM` di dalam tmux:**

```bash
echo $COLORTERM
```

Harusnya keluar `truecolor` atau `24bit`. Kalau **kosong**:

```bash
# ~/.bashrc
export COLORTERM=truecolor

# ~/.tmux.conf
set -ga update-environment "COLORTERM"
```

Reload, lalu buka sesi tmux **baru**.

**Langkah 2 — isolasi masalah: tema vs environment**

Edit `~/.config/superfile/config.toml`, ganti ke tema default:

```toml
theme = "gruvbox"
```

Buka `spf` lagi:
- **Warna normal** → masalahnya di file tema custom (rusak/format salah)
- **Tetap hitam-putih** → masalahnya di environment/tmux, bukan file tema

**Langkah 3 — cek file tema (kalau langkah 2 nunjuk ke tema):**

```bash
cat ~/.config/superfile/theme/tokyonight.toml
```

Pastikan isinya lengkap ada field warna (format hex `#xxxxxx`). Bandingkan strukturnya dengan tema bawaan yang jalan:

```bash
diff ~/.config/superfile/theme/tokyonight.toml ~/.config/superfile/theme/gruvbox.toml
```

**Langkah 4 — cek versi superfile:**

```bash
spf -v
```

Tema custom kadang butuh struktur tertentu sesuai versi superfile. Update kalau perlu:

```bash
bash -c "$(curl -sLo- https://superfile.dev/install.sh)"
```

**Langkah 5 — kalau masih aneh, tambahkan RGB di tmux:**

```bash
set -g default-terminal "tmux-256color"
set -as terminal-features ",*:RGB"
```

### `cd_quit` (Q) tidak mengubah direktori shell

- Pastikan `cd_on_quit = true` di config
- Pastikan pakai **function** `spf()`, bukan binary langsung
- Reload: `source ~/.bashrc`

### Rendering berantakan

- Set locale UTF-8
- Tambah `export RUNEWIDTH_EASTASIAN=0`
- Jangan override `TERM` ke value yang nggak support color

### Konflik hotkey dengan vim/nvim

- Ganti preset hotkey ke versi vim di `hotkeys.toml`
- Lihat peringatan di [GitHub superfile](https://github.com/yorukot/superfile)

### Log error

```bash
cat ~/.local/state/superfile/superfile.log
```

---

## Referensi

- [superfile — Official Site](https://superfile.dev/)
- [Installation](https://superfile.dev/start-here/installation/)
- [Config](https://superfile.dev/configure/superfile-config/)
- [Hotkey List](https://superfile.dev/list/hotkey-list/) → [[Superfile Hotkeys]]
- [Theme List](https://superfile.dev/list/theme-list/)
- [Troubleshooting](https://superfile.dev/troubleshooting/)
- [[install tmux for ubuntu]] — setup tmux dasar
- [[support highlight copas tmux]] — clipboard di tmux
