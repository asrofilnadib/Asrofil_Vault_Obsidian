#Tech #Superfile #TUI #Hotkeys #Terminal

> Referensi default hotkey [superfile](https://superfile.dev/). Semua hotkey bisa di-custom lewat `~/.config/superfile/hotkeys.toml`.
>
> Tekan `?` di dalam superfile untuk buka help menu (hotkey list).

# Table of contents
- [[#General]]
- [[#Panel Navigation]]
- [[#Panel Movement]]
- [[#File Operations]]
- [[#Tips Cepat]]

---

## General

| Fungsi | Key | Variable name |
|--------|-----|---------------|
| Buka superfile | `spf` | — |
| Konfirmasi item terpilih | `enter`, `right`, `l` | `confirm` |
| Keluar dari typing / modal / superfile | `q`, `esc` | `quit` |
| Keluar superfile + `cd` ke folder saat ini | `Q` (shift+q) | `cd_quit` |
| Konfirmasi typing | `enter` | `confirm_typing` |
| Batal typing | `ctrl+c`, `esc` | `cancel_typing` |
| Buka help menu (hotkey list) | `?` | `open_help_menu` |
| Buka prompt di shell mode | `:` | `open_command_line` |
| Buka prompt di spf mode | `>` | `open_spf_prompt` |
| Buka modal navigasi zoxide | `z` | `open_zoxide` |

> `cd_quit` butuh setting `cd_on_quit = true` + wrapper function `spf()` di shell config. Lihat [[Setup Superfile on Ubuntu 24.04]].

---

## Panel Navigation

| Fungsi | Key | Variable name |
|--------|-----|---------------|
| Buat file panel baru | `n` | `create_new_file_panel` |
| Split file panel yang fokus | `N` (shift+n) | `split_file_panel` |
| Tutup file panel yang fokus | `w` | `close_file_panel` |
| Toggle file preview panel | `f` | `toggle_file_preview_panel` |
| Buka menu sort options | `o` | `open_sort_options_menu` |
| Toggle reverse sort | `R` (shift+r) | `toggle_reverse_sort` |
| Toggle footer | `F` (shift+f) | `toggle_footer` |
| Fokus ke file panel berikutnya | `tab`, `L` (shift+l) | `next_file_panel` |
| Fokus ke file panel sebelumnya | `shift+left`, `H` (shift+h) | `previous_file_panel` |
| Fokus ke processbar panel | `p` | `focus_on_process_bar` |
| Fokus ke sidebar | `s` | `focus_on_sidebar` |
| Fokus ke metadata panel | `m` | `focus_on_metadata` |

---

## Panel Movement

| Fungsi | Key | Variable name |
|--------|-----|---------------|
| Naik | `up`, `k` | `list_up` |
| Turun | `down`, `j` | `list_down` |
| Page up | `pgup` | `page_up` |
| Page down | `pgdown` | `page_down` |
| Kembali ke parent folder | `h`, `left`, `backspace` | `parent_directory` |
| Select all (selection mode) | `A` (shift+a) | `file_panel_select_all_items` |
| Select ke atas dari cursor (selection mode) | `shift+up`, `K` (shift+k) | `file_panel_select_mode_items_select_up` |
| Select ke bawah dari cursor (selection mode) | `shift+down`, `J` (shift+j) | `file_panel_select_mode_items_select_down` |
| Toggle tampilan dot file | `.` | `toggle_dot_file` |
| Toggle search bar | `/` | `search_bar` |
| Ganti selection mode / normal mode | `v` | `change_panel_mode` |
| Pin / unpin folder ke sidebar | `P` (shift+p) | `pinned_directory` |

---

## File Operations

| Fungsi | Key | Variable name |
|--------|-----|---------------|
| Buat file / folder (`/` di akhir = folder) | `ctrl+n` | `file_panel_item_create` |
| Rename file / folder | `ctrl+r` | `file_panel_item_rename` |
| Copy item terpilih ke clipboard | `ctrl+c` | `copy_items` |
| Cut item terpilih ke clipboard | `ctrl+x` | `cut_items` |
| Paste dari clipboard | `ctrl+v`, `ctrl+w` | `paste_items` |
| Hapus item terpilih | `ctrl+d`, `delete` | `delete_items` |
| Hapus permanen | `D` (shift+d) | `permanently_delete_items` |
| Copy path file / direktori | `ctrl+p` | `copy_path` |
| Copy current working directory | `c` | `copy_present_working_directory` |
| Extract file terkompresi | `ctrl+e` | `extract_file` |
| Zip file / folder ke `.zip` | `ctrl+a` | `compress_file` |
| Buka file dengan default editor | `e` | `open_file_with_editor` |
| Buka direktori saat ini dengan editor | `E` (shift+e) | `open_current_directory_with_editor` |

---

## Tips Cepat

| Workflow | Kombinasi |
|----------|-----------|
| Navigasi cepat ala vim | `j`/`k` pindah, `h` parent, `l`/`enter` masuk |
| Multi-select lalu operasi bulk | `v` → pilih item → `ctrl+c`/`ctrl+x`/`ctrl+v` |
| Cari file | `/` lalu ketik nama |
| Buka folder tersembunyi | `.` toggle dot file |
| Split panel buat copy antar folder | `N` split → `tab` pindah panel |
| Custom hotkey | Edit `~/.config/superfile/hotkeys.toml` |

> User vim/nvim disarankan pakai preset hotkey vim. Lihat [Custom Hotkeys](https://superfile.dev/configure/custom-hotkeys/).
