#Tech #Tmux #Ubuntu #Clipboard 

# Table of contents
- [[#Introduction]]
- [[#Install Tmux on Ubuntu]]
- [[#Tmux Configuration Script]]
- [[#How to Apply the Configuration]]
- [[#Additional Notes]]

## Introduction

Tmux (Terminal Multiplexer) adalah alat penting bagi developer dan sysadmin untuk mengelola sesi terminal yang persisten, membagi layar menjadi beberapa pane, dan bekerja secara efisien di lingkungan CLI. Di Ubuntu — khususnya Ubuntu 24.04 yang Anda gunakan — tmux bisa diinstal dengan mudah dan dikonfigurasi untuk mendukung fitur modern seperti **highlight-to-copy ke clipboard sistem**, **navigasi mouse**, dan **layout pane fleksibel**.

Berikut panduan lengkap mulai dari instalasi hingga penggunaan konfigurasi kustom Anda.

## Install Tmux on Ubuntu

Untuk menginstal `tmux` di Ubuntu (termasuk Ubuntu 24.04), jalankan perintah berikut di terminal:

```bash
sudo apt update
sudo apt install tmux
```

> 💡 Pastikan juga Anda menginstal alat clipboard sistem sesuai dengan sesi desktop Anda:

**Untuk Wayland** (default di Ubuntu 24.04):
```bash
sudo apt install wl-clipboard
```

**Untuk X11** (jika Anda menggunakan Xorg):
```bash
sudo apt install xclip
```


Setelah terinstal, Anda bisa mulai sesi tmux dengan:
```bash
tmux
```

## Tmux Configuration Script

Simpan konfigurasi berikut di `~/.tmux.conf`. Konfigurasi ini mencakup:
- Navigasi mouse dan scroll
- Copy-paste ke clipboard sistem (dengan `xclip`)
- Keybindings ala Vim
- Pemisahan pane manual dengan kontrol ukuran
- Estetika status bar
- Dukungan plugin (seperti `tmux-resurrect`)

> ⚠️ **Catatan**: Script di bawah adalah **persis seperti yang Anda berikan** — tidak diubah, hanya ditempatkan dalam blok kode untuk dokumentasi.

```bash
# ===========================
# Tmux Configuration - Manual Split
# ===========================

# ===========================
# MOUSE & CLIPBOARD CONFIGURATION
# ===========================
# Enable mouse support
set -g mouse on

# Mouse wheel scrolling
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
bind -n WheelDownPane select-pane -t= \; send-keys -M
bind -n C-WheelUpPane select-pane -t= \; copy-mode -e \; send-keys -M
bind -T copy-mode-vi    C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-vi    C-WheelDownPane send-keys -X halfpage-down
bind -T copy-mode-emacs C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-emacs C-WheelDownPane send-keys -X halfpage-down

# Use vim keybindings in copy mode
setw -g mode-keys vi

# Copy to system clipboard with xclip
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard"
unbind -T copy-mode-vi Enter
bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "xclip -selection clipboard"
bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard"
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi C-v send-keys -X rectangle-toggle

# Enable clipboard integration
set -g set-clipboard on
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "xclip -sel clip -i"
bind -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "xclip -sel clip -i"

# Vi mode for copy mode
setw -g mode-keys vi

# Mouse drag to copy
bind -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -sel clip -i"

# Make Ctrl+a the prefix instead of Ctrl+b
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# Easy pane switching (vim style)
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Reload config
bind r source-file ~/.tmux.conf \; display-message "Reloaded ~/.tmux.conf"

# Pane border colors
set -g pane-border-style fg=brightblack
set -g pane-active-border-style fg=yellow

# Status bar aesthetics
set -g status-bg black
set -g status-fg white
set -g status-left-length 20
set -g status-right-length 60
set -g status-left "#[fg=green]#S #[fg=blue][#I:#P]#[default]"
set -g status-right "#[fg=yellow]%H:%M #[fg=cyan]%d-%b-%Y"

# ===========================
# MANUAL SPLIT KEYBINDINGS
# ===========================

# Unbind default split keys
unbind '"'
unbind %

# Manual split - TANPA auto-layout
# Split kiri-kanan (vertical) 50-50
bind v split-window -h -c "#{pane_current_path}"

# Split atas-bawah (horizontal) 50-50
bind x split-window -v -c "#{pane_current_path}"

# Split dengan ukuran custom 30%
bind V split-window -h -p 30 -c "#{pane_current_path}"
bind X split-window -v -p 30 -c "#{pane_current_path}"

# Split dengan ukuran custom 40%
bind C-v split-window -h -p 40 -c "#{pane_current_path}"
bind C-x split-window -v -p 40 -c "#{pane_current_path}"

# Close pane (simple)
bind q kill-pane

# Resize panes easily
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# Balance layout manually if needed
bind = select-layout tiled

# Load preset layout
bind R run-shell "bash ~/.tmux-preset-layout.sh"

# Plugins (tmux-resurrect)
set -g @plugin 'tmux-plugins/tmux-resurrect'
run '~/.tmux/plugins/tmux-resurrect/resurrect.tmux'
```

## How to Apply the Configuration

1. Simpan script di atas ke file `~/.tmux.conf`:
   ```bash
   nano ~/.tmux.conf
   ```
   Lalu tempelkan isi script dan simpan (`Ctrl+O`, `Enter`, `Ctrl+X`).

2. Jika Anda menggunakan `xclip`, pastikan sudah terinstal (lihat bagian instalasi).

3. Reload konfigurasi tmux:
   ```bash
   tmux source-file ~/.tmux.conf
   ```
   Atau, di dalam sesi tmux, tekan `Ctrl+a` lalu ketik:
   ```
   :source-file ~/.tmux.conf
   ```

   > ⚠️ Karena Anda mengganti prefix ke `Ctrl+a`, pastikan tidak bentrok dengan aplikasi lain (misalnya di GNOME Terminal).

## Additional Notes

- **Wayland Compatibility**: Konfigurasi Anda saat ini menggunakan `xclip`, yang berjalan di X11. Jika Anda ingin full native di **Wayland** (Ubuntu 24.04 default), ganti semua perintah `xclip` dengan `wl-copy`. Contoh:
  ```bash
  bind -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "wl-copy"
  ```
- **Plugin Management**: Jika Anda ingin mengelola plugin tmux (seperti `tmux-resurrect`) dengan lebih mudah, pertimbangkan menggunakan [`tpm` (Tmux Plugin Manager)](https://github.com/tmux-plugins/tpm).
- **Layout Preset**: File `~/.tmux-preset-layout.sh` perlu Anda buat sendiri jika ingin menggunakan binding `R`. Contoh isi:
  ```bash
  #!/bin/bash
  tmux split-window -h -c "#{pane_current_path}"
  tmux split-window -v -c "#{pane_current_path}"
  ```

Dengan konfigurasi ini, tmux Anda siap menjadi pusat workflow terminal yang efisien, intuitif, dan sepenuhnya disesuaikan dengan preferensi Anda.