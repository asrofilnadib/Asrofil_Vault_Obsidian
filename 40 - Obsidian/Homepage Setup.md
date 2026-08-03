# Rainbell Homepage — Setup Notes

Homepage style dari [Rainbell Obsidian-Homepage](https://github.com/Rainbell129/Obsidian-Homepage).

## Setelah buka Obsidian

1. **Enable plugins** — kalau diminta, klik *Enable* untuk plugin baru:
   - Homepage, Banners, Admonition, React Components, Advanced URI, Periodic Notes
2. **Restart Obsidian** sekali biar snippet CSS & plugin ke-load
3. Homepage `00. Homepage.md` akan terbuka otomatis saat startup

## ⚠️ Penting: pakai Reading View

Homepage Rainbell **tidak didesain untuk Source/Edit mode**. Kalau tampilannya masih plain text / properties kelihatan / banner tidak muncul:

1. Buka `00. Homepage.md`
2. Tekan `Ctrl+E` atau klik ikon **buku** (Reading view) di kanan atas
3. Atau tutup Obsidian & buka lagi — plugin Homepage sudah di-set buka **Reading view** otomatis

Shortcut: `Ctrl+Shift+E` toggle antara edit & reading (tergantung keybinding).

## Fitur utama

| Bagian | Fungsi |
|--------|--------|
| **AGENDA** | Daily note, weekly note, task dashboard |
| **LIFE / WORK** | Navigasi ke folder vault lu |
| **Music & Birthdays** | React component — butuh plugin *React Components* |
| **Project Tracking** | Notes dengan tag `#project` + YAML `target: tasks` |
| **Daily Summary** | Ringkasan dari daily notes minggu ini |
| **Recent Activity** | File terbaru di Work/Coding/Personal |

## Project tracking

Tambah di note project:

```yaml
---
target: tasks
status: in progress
tags:
  - project
---
```

Atau word-count based: `target: 10000`

## Birthday countdown

Tambah di note orang:

```yaml
---
birthday: 2003-02-28
---
```

## Tema (opsional)

Rainbell direkomendasikan pakai **Blue Topaz** theme. Lu masih pakai Atom — snippet CSS tetap jalan, tapi kalau mau lebih mirip screenshot asli, install Blue Topaz dari Community Themes.

## Customize

- Banner: edit `banner:` di frontmatter `00. Homepage.md`
- Navigasi: edit link di section AGENDA/LIFE/WORK
- CSS: `.obsidian/snippets/homepage-setting.css`
- React widget: `40 - Obsidian/React components/music and birthday countdown.md`
