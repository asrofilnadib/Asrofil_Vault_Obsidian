# Home server dari PC / laptop bekas — playbook konkret (0 → publik)

> Ini **langkah kerja** berurutan: dari mesin kosor sampai **orang lain bisa buka** satu layanan web lewat domain (pakai **Cloudflare Tunnel**). Basisnya sama tutorial [video ini](https://www.youtube.com/watch?v=z2yBHC_RcDw) + konteks lu (VirtualBox/Debian latihan, VPS = flow sama).
>
> **Lanjutan isi kursus (Jellyfin, Immich, Nextcloud):** [link kursus](https://shafarizkyf.myr.id/course/menggunakan-home-server-install-jellyfin-immich-nextcloud-akses-dimana-saja-tanpa-ip-publik) · **Referensi mini PC di video:** [Shopee](https://s.shopee.co.id/20p26JU7h3)

---

## Map jalur: lu mau sampai mana?

| Target akhir | Yang wajib lu lakuin |
|--------------|----------------------|
| **Server jalan di rumah, lu aja yang remote** | Fase **0 → 6** (sampai **Tailscale**) |
| **Temen/orang lain buka lewat internet (tanpa Tailscale)** | Fase **0 → 10** (+ **domain** + **Tunnel**) |

**Ingat:** **Tailscale** = akses **privat** (yang join VPN). **Cloudflare Tunnel** = **publik** ke satu URL — jangan dipake buat expose SSH/admin sembarangan.

---

## Fase 0 — Cek barang & spek minimum

1. **Mesin:** 64-bit, RAM **minimal 4 GB** (buat Docker lebih nyaman 8 GB+), **virtualization** ON di BIOS kalau nanti butuh VM di dalam server.
2. **Storage:** SSD/NVMe (bekas boleh, baru lebih tenang soal umur tulis).
3. **Flashdisk ≥8 GB** buat installer.
4. **Kabel LAN** + colokan ke router (WiFi bisa, tapi ribet & kurang ideal buat server).
5. **Monitor + keyboard** cuma buat **install pertama**; abis SSH jalan bisa dicabut.

*(VirtualBox/Debian: skip fisik, tapi tetap butuh ISO + VM dengan bridged/NAT dan SSH server.)*

---

## Fase 1 — Download ISO & bikin USB bootable

1. Buka [Ubuntu Server](https://ubuntu.com/download/server) → **Download** ISO.
2. Install **balenaEtcher** di PC lu: [etcher.balena.io](https://etcher.balena.io).
3. Colok flashdisk → Etcher → **Flash from file** (pilih ISO) → **Select target** (flashdisk) → **Flash**.
4. Selesai → **cabut** flashdisk (kadang drive ilang di OS, normal).

---

## Fase 2 — Colok hardware & masuk BIOS

1. Colok **LAN**, **keyboard**, **monitor**, **USB installer**.
2. Nyalain mesin → tekan tombol **BIOS** (beda merk: **F1/F2/Del/Esc** — di video Lenovo: **Enter** lalu **F1**).
3. **Power / AC loss (After power loss):** set ke **Power on** supaya listrik balik mesin nyala sendiri (opsi tergantung BIOS: kadang **Last state**).
4. **Boot order:** **USB** di atas (atau kosongin boot internal dulu kalau disk baru kosong — biasanya otomatis ke USB).
5. **Save & Exit** (umum **F10**).

---

## Fase 3 — Install Ubuntu Server (pilihan yang harus konsisten)

Di installer (urutan kira-kira):

1. **Try or install Ubuntu Server**.
2. Bahasa + keyboard (mis. **English** + layout lu).
3. **Network:** pilih **Ethernet (kabel)** → **Done** (jangan skip kalau bisa; isi WiFi hanya kalau terpaksa).
4. **Storage:** pilih disk server (NVMe/SSD) → layout default → **Continue** kalau yakin **nggak salah pilih disk**.
5. **Profile:** isi **name**, **server name** (hostname), **username**, **password** → catat **username** ini buat SSH.
6. **Ubuntu Pro:** **Skip**.
7. **SSH:** centang **Install OpenSSH server** (penting).
8. **Featured snaps / third party:** bisa **skip** dulu.
9. Tunggu install → **Reboot now**.
10. Kalau muncul **remove installation medium** → **cabut USB** → **Enter**.

**Setelah reboot:** login sekali di layar → catat **IP lokal** yang muncul (format `192.168.x.x`). Itu **bukan** IP internet publik.

---

## Fase 4 — IP tetap di router (DHCP reservation)

Supaya `192.168.x.x` **nggak ganti** tiap reboot:

1. Buka **panel admin router** dari browser (biasanya `192.168.0.1` / `192.168.1.1` — lihat stiker router).
2. Cari menu seperti **DHCP** → **Address Reservation** / **Static DHCP** / **Bind IP to MAC** (nama beda-beda).
3. Pilih device **hostname server** lu atau **MAC address** NIC server → set IP yang sama kayak yang tadi lu catat (atau IP kosong dalam range DHCP yang lu tentuin).
4. Save → **restart server** atau disconnect/reconnect LAN → cek IP masih sama.

**Cek cepat dari PC lain (WiFi/LAN sama router):**

```bash
ping -c 3 192.168.x.x
```

*(Ganti `192.168.x.x` dengan IP server lu.)*

---

## Fase 5 — SSH dari LAN (bukti server “hidup”)

Di **laptop/HP** yang satu jaringan dengan server:

```bash
ssh USERNAME@192.168.x.x
```

- Pertama kali: ketik **`yes`** buat host key.
- Masukin **password** user server.

**Kalau Windows tanpa WSL:** pakai **PowerShell** / **Windows Terminal** / **PuTTY** — sintaks sama `ssh user@ip`.

**Berhasil =** prompt shell kayak `USERNAME@hostname:~$`.

Dari sini monitor-keyboard server **boleh dicabut**; kerja full lewat SSH.

---

## Fase 6 — Tailscale (remote privat: lu dari mana aja)

1. Di browser (HP/PC mana aja): daftar/login **[tailscale.com](https://tailscale.com)**.
2. Di **server** (sudah SSH), ikutin **dokumentasi resmi** install Linux: [Install Tailscale → Linux](https://tailscale.com/download/linux) — biasanya **copy blok perintah** “Ubuntu/Debian” dari situ (lebih aman & mutakhir daripada hafalan script).
3. Setelah terinstall, jalankan:

```bash
sudo tailscale up
```

4. Buka **URL autentikasi** yang muncul di terminal → **Connect** device ke akun lu.
5. Cek di **admin console Tailscale** → mesin server harus **online**.
6. Di halaman mesin itu, catat **Tailscale IP** atau **MagicDNS name** (hostname.tailnet...).

**Tes SSH dari luar rumah (pakai data HP misalnya, HP sudah install app Tailscale + login):**

```bash
ssh USERNAME@100.x.y.z
# atau
ssh USERNAME@nama-mesin.tailxxxxx.ts.net
```

*(Ganti dengan IP/name dari dashboard Tailscale.)*

**Berhasil =** lu bisa admin server **tanpa** buka port SSH ke internet.

---

## Fase 7 — Docker + user lu boleh pakai `docker`

Ikut **official** (pilih distro lu): **[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)** — langkahnya **apt + keyring + repo + install** `docker-ce` dan plugin **compose**.

**Abis install**, masukin user ke grup `docker` (supaya nggak semua perintah harus `sudo`):

```bash
sudo usermod -aG docker "$USER"
```

**Penting:** **logout SSH** lalu **login lagi** (session baru) supaya grup kebaca.

**Verifikasi:**

```bash
docker run --rm hello-world
docker compose version
```

---

## Fase 8 — Satu app jalan di server (contoh: BentoPDF)

Tujuan fase ini: ada proses yang **listen port** (di video **3000**) biar bisa diuji di LAN dulu.

1. Buka repo **BentoPDF** di GitHub → bagian **Self-hosting** → ambil contoh **Docker Compose** (isi file bisa berubah — **selalu ikut yang di repo**, jangan hafal isi lama).
2. Di server:

```bash
mkdir -p ~/docker/bentopdf
cd ~/docker/bentopdf
nano docker-compose.yml
```

3. **Paste** isi compose dari dokumentasi → simpan (**Ctrl+O**, Enter, **Ctrl+X**).
4. Jalankan:

```bash
docker compose up -d
docker ps
```

5. Dari PC di rumah yang **satu LAN**, buka browser:

```text
http://192.168.x.x:PORT
```

*(PORT = yang di `docker-compose.yml`, mis. `3000` atau `8080` — samain.)*

Kalau pakai Tailscale dari luar, ganti host dengan **IP Tailscale** server + port yang sama.

**Kalau error permission Docker:** pastiin Fase 7 (`usermod` + **re-SSH**).

---

## Fase 9 — Domain + arahkan DNS ke Cloudflare

*(Baru butuh kalau lu mau **orang lain** buka lewat **nama domain**, bukan cuma Tailscale.)*

1. Beli domain di registrar (Rumahweb, Hostinger, Niagahoster, dll.) → selesaikan **verifikasi email** kalau diminta.
2. Daftar/login **[dash.cloudflare.com](https://dash.cloudflare.com)** (plan gratis boleh).
3. **Add a Site** → masukin domain lu → ikut wizard sampai Cloudflare kasih **2 (atau lebih) nameserver** (contoh `xxx.ns.cloudflare.com`).
4. Di **panel registrar** (bukan Cloudflare): menu **Nameserver / DNS** → ganti nameserver bawaan → paste nameserver Cloudflare → simpan.
5. Tunggu propagasi (**beberapa menit s/d 24 jam**). Di Cloudflare, status domain jadi **Active** kalau sudah kebaca.

---

## Fase 10 — Cloudflare Tunnel → orang lain bisa akses URL publik

Intinya: program **`cloudflared`** jalan di **server yang sama** dengan Docker → Cloudflare yang nerima traffic HTTPS → diteruskan ke **`localhost:PORT`** di mesin lu. **Ngak perlu** port forwarding di router buat skenario standar ini.

### 10.1 — Install `cloudflared` di server

Cara umum (amd64):

```bash
cd /tmp
curl -fsSL -o cloudflared.deb \
  https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared.deb
```

*(Kalau arsitektur beda, ambil file yang cocok dari halaman release GitHub `cloudflared`.)*

### 10.2 — Buat tunnel di dashboard Cloudflare

1. Cloudflare Dashboard → pilih **domain** lu → menu **Zero Trust** (kadang minta setup sekali).
2. **Networks** → **Tunnels** → **Create a tunnel**.
3. Nama tunnel (bebas, mis. `rumah-ku`) → **Save tunnel**.
4. Pilih OS **Debian** (Ubuntu pakai instruksi yang sama) → **copy** perintah install + perintah **service** yang ada **token** (token **rahasia**, jangan dishare).

5. Di **server**, tempel perintah yang dari dashboard (biasanya mirip):

```bash
sudo cloudflared service install EYJxxxx_TOKEN_PANJANG_DARI_DASHBOARD
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
sudo systemctl status cloudflared
```

**Harusnya:** `active (running)`.

### 10.3 — Publikasi hostname → app lokal

1. Di halaman tunnel yang sama → **Configure** → **Public Hostname** → **Add a public hostname**.
2. **Subdomain:** mis. `bento` → **Domain:** pilih domain lu → hasil: `bento.domainlu.com` / `.id` tergantung beli lu.
3. **Service type:** **HTTP**.
4. **URL:** **`localhost:PORT`** (mis. `localhost:3000`) — **bukan** IP LAN, karena tunnel jalan **di dalam mesin** yang ngebuka port container itu.
5. **Save** → tunggu beberapa detik → buka `https://bento.domainlu...` dari **HP pakai data** (beda jaringan).

**Berhasil =** halaman app kebuka tanpa Tailscale — artinya **publik**. Hati-hati: ini **bukan** layer auth Tailscale.

---

## Verifikasi akhir (checklist)

- [ ] `ping` ke IP LAN reservation **stabil**
- [ ] `ssh user@IP_LAN` OK
- [ ] `ssh user@TAILSCALE_IP` OK dari luar rumah (device sudah Tailscale)
- [ ] `docker ps` → container **Up**
- [ ] Browser: `http://IP_LAN:PORT` OK
- [ ] Browser: `https://subdomain.domain` OK (tanpa VPN)
- [ ] Lu sadar: URL Cloudflare = **siapa saja** bisa coba akses → pertimbangkan auth di app / jangan expose admin

---

## Timestamp video (kalau mau nonton per segmen)

| Waktu | Topik |
|------|--------|
| 00:00–00:29 | Intro |
| 00:30–01:25 | Belanja |
| 01:26–01:47 | Spesifikasi |
| 01:48–03:26 | Pasang NVMe |
| 03:27–05:34 | Bootable (Etcher) |
| 05:35–05:59 | Persiapan install |
| 06:00–13:04 | Install OS |
| 13:05–14:12 | DHCP reservation |
| 14:13–15:11 | SSH LAN |
| 15:12–18:40 | Tailscale |
| 18:41–20:13 | Docker |
| 20:14–24:54 | BentoPDF |
| 24:55–27:40 | Beli domain |
| 27:41–30:10 | Cloudflare DNS |
| 30:11–33:28 | Tunnel |

---

## VPS / VirtualBox — bedanya cuma di “Fase 0–3”

| Konteks | Yang disederhanakan |
|---------|---------------------|
| **VPS** | Provider kasih ISO/console + **IP publik** SSH; **skip** USB & BIOS fisik; **reservasi DHCP** diganti IP statis dari panel VPS kalau perlu. |
| **VirtualBox** | ISO Debian/Ubuntu di-mount VM; pastikan **adapter bridged** atau **NAT + port forward 22** biar SSH dari host; install `openssh-server` kalau image minimal. |

Sisanya: **SSH → Tailscale → Docker → (opsional) Tunnel** sama persis.

---

## Tag

#home-server #selfhosted #tailscale #cloudflare #docker #ubuntu #playbook #obsidian-work
