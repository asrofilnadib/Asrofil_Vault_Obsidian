# Ubuntu 24.04 — Gesture swipe (Touchégg + X11)

> Panduan ini buat **pindah workspace** pakai **3 jari** di Ubuntu 24.04 (noble). PPA `ppa:touchegg/stable` sering gagal di noble, jadi install **manual dari `.deb` GitHub** (sesuai screenshot lu).

---

## Langkah 1: Pastikan Touchégg terinstall & jalan

### Kenapa manual?

Di Ubuntu 24.04, PPA `ppa:touchegg/stable` sering nggak jalan. Solusinya: **download `.deb` dari [GitHub Releases](https://github.com/JoseExposito/touchegg/releases)** terus install lewat `apt`/`dpkg`.

### 1. Download paket `.deb`

Buka releases:

👉 https://github.com/JoseExposito/touchegg/releases

Cari file **`touchegg_<versi>_amd64.deb`** (contoh yang pernah dipakai dan **Latest** di screenshot lu: **`touchegg_2.0.18_amd64.deb`**). Kalau nanti ada versi lebih baru, ganti nomor versi di URL dan nama file.

Contoh download lewat terminal (ganti folder kalau mau):

```bash
cd ~/Downloads
wget https://github.com/JoseExposito/touchegg/releases/download/2.0.18/touchegg_2.0.18_amd64.deb
```

> **Catatan:** Kalau nggak ada build khusus “noble”, paket **amd64** buat Debian/Ubuntu biasanya tetap oke; kalau ragu, ambil release terbaru yang ada aset `.deb` amd64.

### 2. Install `.deb`

Dari folder tempat `.deb` ada (misalnya `~/Downloads`):

```bash
cd ~/Downloads   # atau path lain tempat file .deb
sudo apt-get install ./touchegg_*.deb
```

Atau pakai nama file penuh:

```bash
sudo apt-get install ./touchegg_2.0.18_amd64.deb
```

Kalau ada **missing dependency** / broken install:

```bash
sudo apt-get install -f
```

### 3. Jalankan Touchégg

Manual (foreground):

```bash
touchegg
```

Atau **enable & start** sebagai service (coba yang cocok sama paket lu — seringnya **user service**):

```bash
systemctl --user enable touchegg
systemctl --user start touchegg
systemctl --user status touchegg
```

Kalau yang kepasang **service sistem**:

```bash
sudo systemctl enable touchegg
sudo systemctl start touchegg
sudo systemctl status touchegg
```

Harusnya status: **`active (running)`**.

---

## Langkah 2: Konfigurasi gesture pindah workspace

### 1. Folder & salin config default

```bash
mkdir -p ~/.config/touchegg
sudo cp /usr/share/touchegg/touchegg.conf ~/.config/touchegg/touchegg.conf
sudo chown "$USER:$USER" ~/.config/touchegg/touchegg.conf
```

> Kalau path default beda di mesin lu, cari file contoh: `find /usr -name touchegg.conf 2>/dev/null`

### 2. Edit config

```bash
nano ~/.config/touchegg/touchegg.conf
```

### 3. Tambah gesture di `<application name="All">`

Cari blok **`<application name="All">`** … **`</application>`**. Di **dalam** blok itu (biasanya bareng gesture default lain), tambahin **satu** dari dua opsi di bawah.

#### Opsi A — `SEND_KEYS` (paling simpel, pakai shortcut GNOME)

Ini ngebaca **shortcut bawaan umum** “workspace kanan/kiri” pakai **Super + Page Down / Page Up**. Kalau di mesin lu shortcutnya beda, ubah di **Settings → Keyboard → View and Customize Shortcuts →** bagian workspace / navigation, lalu samain key-nya di sini.

**Mapping sesuai tes lu:**

- Geser **3 jari ke kiri** → workspace **kanan** (ikut arah “next”)
- Geser **3 jari ke kanan** → workspace **kiri**

```xml
    <gesture type="SWIPE" fingers="3" direction="LEFT">
      <action type="SEND_KEYS">
        <repeat>false</repeat>
        <modifiers>Super_L</modifiers>
        <keys>Page_Down</keys>
      </action>
    </gesture>
    <gesture type="SWIPE" fingers="3" direction="RIGHT">
      <action type="SEND_KEYS">
        <repeat>false</repeat>
        <modifiers>Super_L</modifiers>
        <keys>Page_Up</keys>
      </action>
    </gesture>
```

#### Opsi B — `RUN_COMMAND` + `xdotool` (kalau shortcut beda)

Install dulu kalau belum:

```bash
sudo apt-get install xdotool
```

Contoh (sama arahnya kaya di atas — sesuaikan key combo sama setting keyboard lu):

```xml
    <gesture type="SWIPE" fingers="3" direction="LEFT">
      <action type="RUN_COMMAND">
        <command>xdotool key Super+Page_Down</command>
      </action>
    </gesture>
    <gesture type="SWIPE" fingers="3" direction="RIGHT">
      <action type="RUN_COMMAND">
        <command>xdotool key Super+Page_Up</command>
      </action>
    </gesture>
```

Simpan di nano: **Ctrl+O**, Enter, lalu **Ctrl+X**.

### 4. Restart Touchégg

```bash
killall touchegg 2>/dev/null; touchegg &
```

Atau lewat systemd (user):

```bash
systemctl --user restart touchegg
```

(system):

```bash
sudo systemctl restart touchegg
```

---

## Langkah 3: Extension GNOME X11 Gestures

Lu udah install **x11gestures** — yang krusial:

1. **Session harus X11, bukan Wayland.**  
   Di layar login: ikon **gear** → pilih **“Ubuntu on Xorg”** (atau varian X11).

2. Cek dari terminal:

```bash
echo $XDG_SESSION_TYPE
```

Harus keluar: **`x11`**

Extension **x11gestures** cuma relevan di **X11**.

3. Setelah install/update extension, restart GNOME Shell: **Alt+F2** → ketik **`r`** → Enter.

---

## Langkah 4: Tes gesture

- **3 jari ke kiri** → workspace **kanan**
- **3 jari ke kanan** → workspace **kiri**

Kalau belum ngaruh:

```bash
journalctl -u touchegg -f
```

Atau (kalau pakai user service):

```bash
journalctl --user -u touchegg -f
```

Hindari bentrok sama **libinput-gestures** atau **fusuma** (nonaktifin salah satu kalau kebetulan keduanya jalan).

---

## Referensi cepat

| Item | Nilai |
|------|--------|
| Releases | https://github.com/JoseExposito/touchegg/releases |
| Contoh `.deb` (screenshot) | `touchegg_2.0.18_amd64.deb` |
| Config user | `~/.config/touchegg/touchegg.conf` |
| Cek session | `echo $XDG_SESSION_TYPE` → `x11` |

#ubuntu #touchegg #gesture #x11 #obsidian-work
