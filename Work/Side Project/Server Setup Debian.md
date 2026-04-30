# 🖥️ Server Setup Debian

> Panduan lengkap setup server Debian dengan Tailscale, Docker, Portainer, Beszel, Jellyfin, Immich, dan Nextcloud AIO

---

## 📋 Daftar Isi

- [[#🛠️ Prerequisites|Prerequisites]]
- [[#🔐 Tailscale Configuration|Tailscale Configuration]]
- [[#🐳 Docker Installation|Docker Installation]]
- [[#📊 Portainer Setup|Portainer Setup]]
- [[#📦 Container Setup|Container Setup]]
- [[#🔧 Troubleshooting|Troubleshooting]]
- [[#💾 Storage Management|Storage Management]]

---

## 🛠️ Prerequisites

Sebelum mulai, pastikan lu punya:
- ✅ Debian Bookworm (12) atau Bullseye (11) - recommended Bookworm
- ✅ Akses root atau sudo
- ✅ Koneksi internet stabil
- ✅ Akun Tailscale (daftar di [tailscale.com](https://tailscale.com))

---

## 🔐 Tailscale Configuration

### Instalasi Tailscale

```bash
# 1. Tambahin signing key dan repository Tailscale
curl -fsSL https://pkgs.tailscale.com/stable/debian/bookworm.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null
curl -fsSL https://pkgs.tailscale.com/stable/debian/bookworm.tailscale-keyring.list | sudo tee /etc/apt/sources.list.d/tailscale.list

# 2. Update package list
sudo apt-get update

# 3. Install Tailscale
sudo apt-get install tailscale

# 4. Connect ke Tailnet lu
sudo tailscale up
```

**Catatan:** Pas jalanin `tailscale up`, lu bakal dikasih link buat autentikasi. Buka link itu di browser dan login pake akun Tailscale lu.

### Konfigurasi Tailscale Admin Console

Sebelum setup Nextcloud, lu perlu konfigurasi beberapa hal di [Tailscale Admin Console](https://login.tailscale.com/admin):

#### 1. Copy Tailnet Domain Name
- Buka [DNS Settings](https://login.tailscale.com/admin/dns)
- Copy domain name lu (format: `{tailnetdomain}.ts.net`)
- Simpen buat dipake nanti

#### 2. Enable HTTPS Certificates
- Masih di [DNS Settings](https://login.tailscale.com/admin/dns)
- **Enable HTTPS certificates** (defaultnya disabled)
- Ini penting buat Nextcloud nanti

#### 3. Buat Tag di ACL
- Buka [ACL Editor](https://login.tailscale.com/admin/acls/file)
- Tambahin tag `nextcloud` di bagian `tagOwners`:
```json
"tagOwners": {
    "tag:nextcloud": ["autogroup:admin"],
}
```

#### 4. Generate OAuth Client
- Buka [OAuth Settings](https://login.tailscale.com/admin/settings/oauth)
- Klik **"Generate OAuth client"**
- Pilih **"Auth Keys: Write"** di bagian Devices
- **PENTING:** Pastikan lu pilih tag `nextcloud` yang udah lu buat tadi
- Copy credentials yang di-generate (format: `tskey-client-xxxxx`)
- Simpen aman-aman, ini bakal dipake di docker-compose nanti

### Skenario Troubleshooting Tailscale

| Masalah | Solusi |
|---------|--------|
| Tailscale ga connect | `sudo systemctl status tailscaled` - cek status service |
| Mau logout/reset | `sudo tailscale logout` - logout dari Tailnet |
| Cek IP Tailscale | `tailscale ip -4` - liat IPv4 lu di Tailnet |
| Cek status detail | `tailscale status` - liat semua peer yang connected |
| Restart service | `sudo systemctl restart tailscaled` - restart Tailscale daemon |
| Cek route | `tailscale status --peers` - liat routing table |
| Ping peer lain | `ping <tailscale-ip>` - test koneksi ke peer lain |

**Tips:**
- Kalo Tailscale ga connect, cek firewall lu dulu: `sudo ufw status` atau `sudo iptables -L`
- Kalo masih ga bisa, coba restart: `sudo systemctl restart tailscaled && sudo tailscale up`
- Buat cek apakah host lu udah masuk Tailnet: buka [Admin Console](https://login.tailscale.com/admin/machines) dan cari nama host lu

---

## 🐳 Docker Installation

### Install Docker Engine

```bash
# 1. Uninstall versi lama (kalo ada)
sudo apt-get remove docker docker-engine docker.io containerd runc

# 2. Update package index
sudo apt-get update

# 3. Install dependencies
sudo apt-get install ca-certificates curl gnupg lsb-release

# 4. Tambahin Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# 5. Setup repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 6. Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 7. Verify installation
sudo docker run hello-world
```

### Post-Installation Setup

```bash
# Tambahin user ke docker group (biar ga perlu sudo terus)
sudo usermod -aG docker $USER

# Logout dan login lagi, atau:
newgrp docker

# Test tanpa sudo
docker run hello-world

# Enable Docker buat start otomatis pas boot
sudo systemctl enable docker
sudo systemctl start docker
```

### Skenario Troubleshooting Docker

| Masalah | Solusi |
|---------|--------|
| Permission denied | `sudo usermod -aG docker $USER` lalu logout/login |
| Docker ga jalan | `sudo systemctl status docker` - cek status |
| Restart Docker | `sudo systemctl restart docker` |
| Cek versi | `docker --version` dan `docker compose version` |
| List containers | `docker ps -a` - liat semua container |
| List images | `docker images` - liat semua image |
| Clean up | `docker system prune -a` - hapus yang ga kepake (hati-hati!) |
| Cek logs | `docker logs <container-name>` - liat log container |

**Tips:**
- Kalo container ga jalan, cek log dulu: `docker logs <container-name>`
- Buat liat resource usage: `docker stats`
- Buat stop semua container: `docker stop $(docker ps -q)`

---

## 📊 Portainer Setup

Portainer itu web UI buat manage Docker containers. Enak banget buat yang ga suka CLI.

### Install Portainer dengan Docker Compose

Buat folder buat Portainer:
```bash
mkdir -p ~/docker/portainer
cd ~/docker/portainer
```

**Cara 1: Download compose file resmi**
```bash
curl -L https://downloads.portainer.io/ce-sts/portainer-compose.yaml -o portainer-compose.yaml
docker compose -f portainer-compose.yaml up -d
```

**Cara 2: Buat manual**

Buat file `docker-compose.yml`:
```yaml
services:
  portainer:
    container_name: portainer
    image: portainer/portainer-ce:sts
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
    ports:
      - 9443:9443   # HTTPS Web UI
      - 8000:8000   # Portainer API (optional, buat Edge Agents)

volumes:
  portainer_data:
    name: portainer_data

networks:
  default:
    name: portainer_network
```

Jalankan:
```bash
docker compose up -d
```

### Akses Portainer

Setelah container jalan, akses Portainer di:
- **HTTPS:** `https://localhost:9443` atau `https://<server-ip>:9443` atau `https://<tailscale-ip>:9443`

Pas pertama kali buka, lu bakal diminta bikin admin user dan password.

**Catatan:**
- Port 8000 cuma perlu kalo lu mau pake Edge compute features
- Portainer generate self-signed SSL certificate otomatis buat port 9443
- Kalo butuh HTTP port 9000 (legacy), tambahin `-p 9000:9000` di ports

### Storage Portainer

Portainer nyimpen datanya di volume `portainer_data`. Data ini termasuk:
- User accounts
- Endpoints configuration
- Stacks/Compose files
- Settings

**Lokasi:** Data tersimpan di Docker volume, bisa di-backup dengan:
```bash
docker run --rm -v portainer_data:/data -v $(pwd):/backup alpine tar czf /backup/portainer-backup-$(date +%Y%m%d).tar.gz /data
```

**Update Portainer:**
```bash
cd ~/docker/portainer

# Pull latest image
docker compose pull

# Restart dengan versi baru
docker compose up -d
```

**Tips:**
- Portainer generate self-signed SSL certificate otomatis buat port 9443
- Kalo mau pake SSL certificate sendiri, bisa setup via Portainer UI setelah instalasi
- Port 8000 cuma perlu kalo lu mau pake Edge Agents (optional)
- Portainer bakal auto-update kalo lu set label `io.portainer.update` di container

---

## 📦 Container Setup

### 🎬 Jellyfin Container

Jellyfin buat media server - streaming film, series, musik, dll.

#### Docker Compose untuk Jellyfin

Buat folder:
```bash
mkdir -p ~/docker/jellyfin
cd ~/docker/jellyfin
```

Buat `docker-compose.yml`:
```yaml
services:
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    # Optional - specify uid:gid kalo mau run sebagai user tertentu
    # user: uid:gid
    ports:
      - 8096:8096/tcp   # Web UI
      - 7359:7359/udp   # Auto-discovery (optional)
    volumes:
      - /path/to/config:/config
      - /path/to/cache:/cache
      - type: bind
        source: /path/to/media
        target: /media
    restart: unless-stopped
    # Optional - alternative address untuk autodiscovery
    environment:
      - JELLYFIN_PublishedServerUrl=https://jellyfin.your-tailnet.ts.net
```

**Catatan Penting:**
- Ganti `/path/to/config`, `/path/to/cache`, dan `/path/to/media` dengan lokasi yang sesuai
- Kalo punya beberapa media library, tambahin bind mount lagi:
```yaml
volumes:
  - /path/to/config:/config
  - /path/to/cache:/cache
  - type: bind
    source: /mnt/storage/movies
    target: /media/movies
    read_only: true
  - type: bind
    source: /mnt/storage/tvshows
    target: /media/tvshows
    read_only: true
```

**Tips:**
- Kalo mau pake hardware acceleration, perlu mount `/dev/dri` device
- Port 7359 UDP buat DLNA auto-discovery (optional)
- Kalo mau pake custom fonts buat subtitle burn-in, mount folder fonts juga

#### Storage Jellyfin

| Tipe Data | Lokasi | Ukuran Estimasi |
|-----------|--------|-----------------|
| Config | `./config` | 50MB - 500MB |
| Cache | `./cache` | 1GB - 10GB (tergantung library) |
| Metadata | `./config/metadata` | Bisa besar banget (tergantung jumlah media) |
| Media Files | `/path/to/media` | Tergantung koleksi lu |

**Rekomendasi:**
- Config dan cache: simpen di SSD (buat performa)
- Media files: simpen di HDD besar (buat kapasitas)
- Backup config folder secara berkala (metadata penting!)

**Akses:**
- Local: `http://localhost:8096`
- Via Tailscale: `http://<tailscale-ip>:8096` atau setup reverse proxy

**Update Jellyfin:**
```bash
cd ~/docker/jellyfin

# Pull latest image
docker compose pull

# Restart dengan versi baru
docker compose up -d
```

---

### 📸 Immich Container

Immich buat self-hosted photo management - kayak Google Photos tapi private.

#### Docker Compose untuk Immich

Immich butuh beberapa service (database, Redis, dll). Download compose file resmi dari releases:

```bash
mkdir -p ~/docker/immich
cd ~/docker/immich

# Download docker-compose.yml dari latest release
wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml

# Download .env file dari latest release
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
```

**Edit file `.env`** - set lokasi storage dan password:
```env
# Lokasi upload files (foto/video)
UPLOAD_LOCATION=./library

# Lokasi database files (network shares tidak didukung!)
DB_DATA_LOCATION=./postgres

# Timezone (optional, uncomment dan set)
# TZ=Asia/Jakarta

# Immich version (bisa pin ke versi spesifik seperti "v2.1.0")
IMMICH_VERSION=v2

# Database password (GANTI dengan password yang kuat!)
# Hanya pakai karakter A-Za-z0-9, tanpa special characters
DB_PASSWORD=postgres

# Values di bawah ini tidak perlu diubah
DB_USERNAME=postgres
DB_DATABASE_NAME=immich
```

**Tips:**
- `DB_PASSWORD` harus diganti dengan password yang kuat (pake `pwgen` kalo perlu)
- `UPLOAD_LOCATION` harus folder baru dengan space yang cukup
- `DB_DATA_LOCATION` tidak bisa pakai network shares

Jalankan:
```bash
docker compose up -d
```

#### Storage Immich

| Tipe Data | Lokasi | Ukuran Estimasi |
|-----------|--------|-----------------|
| Uploaded Photos/Videos | `./library` | Tergantung jumlah foto/video |
| Thumbnails | `./library/thumbs` | ~5-10% dari ukuran original |
| Encoded Videos | `./library/encoded-video` | Bisa besar (transcoded videos) |
| Database | `./postgres` | Relatif kecil (~100MB - 1GB) |
| Profile Data | `./library/profile` | < 1MB |

**Rekomendasi:**
- Library folder: simpen di storage besar (HDD/SSD besar)
- Database: simpen di SSD (buat performa query)
- Backup library folder secara berkala (foto/video penting!)

**Akses:**
- Local: `http://localhost:2283`
- Via Tailscale: `http://<tailscale-ip>:2283`

**Update Immich:**
```bash
cd ~/docker/immich

# Pull latest images
docker compose pull

# Restart dengan versi baru
docker compose up -d
```

**Tips:**
- Immich bakal scan semua foto/video di library folder
- Proses scan bisa lama kalo library-nya besar
- Monitor disk space - Immich bakal track available space di `UPLOAD_LOCATION`
- Kalo dapet error `can't set healthcheck.start_interval as feature require Docker Engine v25 or later`, comment out line `start_interval` di section `database` di `docker-compose.yml`
- Jangan mount `upload/` dan `library/` folder secara terpisah kalo mereka di device yang sama (karena Docker bind mount properties)
- Restart Immich setelah ubah config: `docker compose restart` atau `docker compose up -d`

---

### 📁 Beszel Container

Beszel itu monitoring tool buat server - tracking system resources, health checks, notifications, dll.

#### Docker Compose untuk Beszel

Buat folder:
```bash
mkdir -p ~/docker/beszel
cd ~/docker/beszel
```

Buat `docker-compose.yml`:
```yaml
services:
  beszel:
    image: henrygd/beszel
    container_name: beszel
    restart: unless-stopped
    environment:
      - APP_URL=http://localhost:8090  # Ganti dengan URL lu (domain atau IP)
    ports:
      - 8090:8090
    volumes:
      - ./beszel_data:/beszel_data
```

Jalankan:
```bash
docker compose up -d
```

**Catatan:** Ganti `APP_URL` dengan URL yang bakal lu pake buat akses Beszel. Contoh:
- Kalo via Tailscale: `APP_URL=http://beszel.your-tailnet.ts.net:8090`
- Kalo via local IP: `APP_URL=http://192.168.1.100:8090`

#### Storage Beszel

| Tipe Data | Lokasi | Ukuran Estimasi |
|-----------|--------|-----------------|
| Config & Data | `./beszel_data` | Relatif kecil (~10-100MB) |

**Akses:**
- Local: `http://localhost:8090`
- Via Tailscale: `http://<tailscale-ip>:8090` atau setup reverse proxy

**Update Beszel:**
```bash
cd ~/docker/beszel

# Pull latest image
docker compose pull

# Restart dengan versi baru
docker compose up -d
```

**Tips:**
- Beszel bisa monitor Docker containers, system resources, disk usage, dll
- Bisa setup notifications ke berbagai platform (Discord, Telegram, dll)
- Check [Beszel Docs](https://www.beszel.dev/guide/) buat konfigurasi lebih lanjut
- Kalo mau pake Beszel Agent (buat monitor host), perlu setup agent container terpisah

---

### ☁️ Nextcloud AIO dengan Tailscale

Nextcloud AIO (All-in-One) itu versi yang udah include semua yang dibutuhkan. Setup-nya agak ribet karena perlu integrasi dengan Tailscale dan Caddy sebagai reverse proxy.

#### Prerequisites

**PENTING BANGET:** Pastikan lu udah:
- ✅ **Install Tailscale di HOST Debian dulu!** Ini wajib - host harus connected ke Tailnet sebelum setup Nextcloud AIO
- ✅ Setup Tailscale di host (lihat bagian [[#🔐 Tailscale Configuration|Tailscale Configuration]])
- ✅ Generate OAuth Client di Tailscale Admin Console
- ✅ Enable HTTPS Certificates di Tailscale
- ✅ Buat tag `nextcloud` di ACL

**Kenapa host harus di Tailnet?**
- Container Nextcloud perlu resolve domain Tailscale dari dalam container
- Kalo host ga di Tailnet, DNS resolution bakal gagal dan container ga bisa connect
- Ini salah satu masalah paling umum yang bikin setup gagal!

#### Setup Nextcloud AIO

Buat folder:
```bash
mkdir -p ~/docker/nextcloud-aio
cd ~/docker/nextcloud-aio
```

Buat `compose.yml`:
```yaml
services:
  nextcloud-aio-mastercontainer:
    image: ghcr.io/nextcloud-releases/all-in-one:beta
    init: true
    restart: always
    container_name: nextcloud-aio-mastercontainer
    volumes:
      - nextcloud_aio_mastercontainer:/mnt/docker-aio-config
      - /var/run/docker.sock:/var/run/docker.sock:ro
    networks:
      - nextcloud-aio
    ports:
      - 0.0.0.0:8080:8080
    environment:
      APACHE_PORT: 11000
      APACHE_IP_BINDING: 127.0.0.1
      SKIP_DOMAIN_VALIDATION: true

  caddy:
    build:
      context: .
      dockerfile: Caddy.Dockerfile
    depends_on:
      tailscale:
        condition: service_healthy
    restart: unless-stopped
    environment:
      NC_DOMAIN: nextcloud.your-tailnet.ts.net  # GANTI dengan domain lu!
    volumes:
      - type: bind
        source: ./Caddyfile
        target: /etc/caddy/Caddyfile
      - type: volume
        source: caddy_certs
        target: /certs
      - type: volume
        source: caddy_data
        target: /data
      - type: volume
        source: caddy_config
        target: /config
      - type: volume
        source: tailscale_sock
        target: /var/run/tailscale/
        read_only: true
    network_mode: service:tailscale

  tailscale:
    image: tailscale/tailscale:v1.82.0
    environment:
      TS_HOSTNAME: nextcloud  # Hostname di Tailnet
      TS_AUTH_KEY: tskey-client-xxxxx  # GANTI dengan OAuth key lu!
      TS_EXTRA_ARGS: --advertise-tags=tag:nextcloud
    init: true
    healthcheck:
      test: tailscale status --peers=false --json | grep 'Online.*true'
      start_period: 3s
      interval: 1s
      retries: 3
    restart: unless-stopped
    devices:
      - /dev/net/tun:/dev/net/tun
    volumes:
      - type: volume
        source: tailscale
        target: /var/lib/tailscale
      - type: volume
        source: tailscale_sock
        target: /tmp
    cap_add:
      - NET_ADMIN
    networks:
      - nextcloud-aio

volumes:
  nextcloud_aio_mastercontainer:
    name: nextcloud_aio_mastercontainer
  caddy_certs:
  caddy_config:
  caddy_data:
  tailscale:
  tailscale_sock:

networks:
  nextcloud-aio:
    name: nextcloud-aio
    driver: bridge
    enable_ipv6: false
    driver_opts:
      com.docker.network.driver.mtu: "1280"
      com.docker.network.bridge.host_binding_ipv4: "127.0.0.1"
      com.docker.network.bridge.enable_icc: "true"
      com.docker.network.bridge.default_bridge: "false"
      com.docker.network.bridge.enable_ip_masquerade: "true"
```

Buat `Caddyfile`:
```caddyfile
{
    layer4 {
        127.0.0.1:3478 {
            route {
                proxy {
                    upstream nextcloud-aio-talk:3478
                }
            }
        }
        127.0.0.1:3479 {
            route {
                proxy {
                    upstream nextcloud-aio-talk:3479
                }
            }
        }
    }
}

https://{$NC_DOMAIN} {
    reverse_proxy nextcloud-aio-apache:11000 {
        header_up X-Forwarded-Proto "https"
        header_up Host {host}
    }
}

http://{$NC_DOMAIN} {
    reverse_proxy nextcloud-aio-apache:11000 {
        header_up X-Forwarded-Proto "http"
        header_up Host {host}
    }
}
```

Buat `Caddy.Dockerfile`:
```dockerfile
FROM caddy:2.9.1-builder-alpine AS builder
RUN xcaddy build --with github.com/mholt/caddy-l4@87e3e5e2c7f986b34c0df373a5799670d7b8ca03

FROM caddy:2.9.1-alpine
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```

#### Deploy Nextcloud AIO

```bash
# Build dan start containers
docker compose up --build --pull always --wait

# Monitor logs
docker compose logs --follow

# Akses AIO interface
# Buka: https://<server-ip>:8080/
```

Pas pertama kali, lu bakal diminta:
1. Masukin domain yang udah lu set di `NC_DOMAIN` (format: `nextcloud.your-tailnet.ts.net`)
2. Ikutin setup wizard
3. Set admin user dan password

**PENTING:** Domain yang lu masukin harus sesuai dengan **ephemeral hostname** yang dibuat Tailscale, bukan hostname fisik server lu. Cek di [Tailscale Admin Console](https://login.tailscale.com/admin/machines) buat liat hostname yang bener.

Setelah setup selesai, akses Nextcloud di: `https://nextcloud.your-tailnet.ts.net`

**Catatan:** Kalo setup domain salah pertama kali, lu harus reset semua volumes karena domain tidak bisa diubah setelah setup awal.

#### Storage Nextcloud

| Tipe Data | Lokasi | Ukuran Estimasi |
|-----------|--------|-----------------|
| User Files | `/mnt/ncdata` (default) | Tergantung penggunaan |
| Database | Internal di container | Relatif kecil |
| Config | Volume `nextcloud_aio_mastercontainer` | ~100MB - 1GB |
| Apps/Plugins | Internal di container | ~500MB - 2GB |

**Rekomendasi:**
- User files: simpen di storage besar (bisa di external drive)
- Buat ubah lokasi data dir, set environment variable `NEXTCLOUD_DATADIR` di `compose.yml`
- Backup data folder secara berkala!

**Akses:**
- AIO Interface: `https://<server-ip>:8080`
- Nextcloud Web: `https://nextcloud.your-tailnet.ts.net`
- Via Tailscale: akses dari device yang connected ke Tailnet

#### Troubleshooting Nextcloud AIO

| Masalah | Solusi |
|---------|--------|
| Container ga connect | **Pastikan host juga di Tailnet!** Install Tailscale di host Debian dulu |
| Domain ga resolve | Cek di Tailscale Admin Console, pastikan machine muncul. Cek juga hostname yang dipake |
| Apache unhealthy | Cek log: `docker logs nextcloud-aio-apache`. Biasanya karena DNS resolution issue |
| cURL error 6: Could not resolve host | Host belum di Tailnet atau domain format salah. Pastikan host connected ke Tailnet |
| Caddy error | Cek `Caddyfile` - pastikan `{$NC_DOMAIN}` format benar (jangan manual replace!) |
| OAuth key invalid | Generate ulang di Tailscale Admin Console. Pastikan pilih tag `nextcloud` |
| Ephemeral hostname issue | Hostname yang dipake adalah yang dibuat Tailscale, bukan hostname fisik server |
| Collabora ga jalan | Pastikan host juga di Tailnet. Restart containers setelah host connected |
| Container restart terus | Cek OAuth key, ACL settings, dan pastikan network `nextcloud-aio` dibuat dengan benar |

**Skenario Troubleshooting Detail:**

**Kalo dapet error "Could not resolve host":**
```bash
# 1. Pastikan host di Tailnet
sudo tailscale status

# 2. Cek DNS resolution dari host
nslookup nextcloud.your-tailnet.ts.net

# 3. Cek DNS resolution dari dalam container
docker exec -it nextcloud-aio-nextcloud nslookup nextcloud.your-tailnet.ts.net

# 4. Kalo masih ga bisa, reset network
docker compose down
docker network rm nextcloud-aio
docker compose up -d
```

**Kalo Apache container unhealthy:**
```bash
# Cek healthcheck manual
docker exec -it nextcloud-aio-apache bash -x /healthcheck.sh

# Biasanya masalahnya di DNS resolution ke domain Tailscale
# Pastikan host connected ke Tailnet!
```

**Tips Penting:**
- **Host HARUS di Tailnet juga!** Install Tailscale di host Debian lu dulu sebelum setup Nextcloud AIO
- Domain format harus: `{hostname}.{tailnet-domain}.ts.net` (contoh: `nextcloud.tail6c5f4d.ts.net`)
- Hostname yang dipake adalah **ephemeral hostname** yang dibuat Tailscale, bukan hostname fisik server lu
- Kalo container restart terus, cek OAuth key dan ACL settings
- Kalo domain ga resolve dari dalam container, pastikan host juga connected ke Tailnet
- Buat reset total: `docker compose down --volumes` (hati-hati, hapus semua data!)
- Kalo Apache container unhealthy, biasanya karena DNS resolution issue - pastikan host di Tailnet

---

## 🔧 Troubleshooting

### Checklist Umum

- [ ] Cek status semua service: `systemctl status <service-name>`
- [ ] Cek log container: `docker logs <container-name>`
- [ ] Cek disk space: `df -h`
- [ ] Cek memory: `free -h`
- [ ] Cek network: `ip addr` atau `ifconfig`
- [ ] Test koneksi: `ping <ip>` atau `curl <url>`

### Command Cheat Sheet

```bash
# Docker
docker ps -a                    # List semua container
docker logs <container>         # Log container
docker restart <container>      # Restart container
docker stop <container>         # Stop container
docker start <container>        # Start container
docker exec -it <container> bash # Masuk ke container

# Tailscale
tailscale status                # Status Tailscale
tailscale ip -4                 # IPv4 address
sudo tailscale logout           # Logout
sudo systemctl restart tailscaled # Restart service

# System
sudo systemctl status <service> # Cek status service
sudo systemctl restart <service> # Restart service
df -h                           # Disk usage
free -h                         # Memory usage
htop                            # Resource monitor
```

---

## 💾 Storage Management

### Rekomendasi Struktur Storage

```
/mnt/
├── storage/           # Main storage (HDD besar)
│   ├── media/         # Jellyfin media files
│   │   ├── movies/
│   │   ├── tvshows/
│   │   └── music/
│   ├── photos/        # Immich library
│   └── nextcloud/     # Nextcloud data
│
└── ssd/               # SSD untuk config/cache (optional)
    ├── docker/
    │   ├── jellyfin/config
    │   ├── jellyfin/cache
    │   └── immich/postgres
```

### Backup Strategy

**Jellyfin:**
```bash
# Backup config (metadata penting!)
tar czf jellyfin-config-backup-$(date +%Y%m%d).tar.gz ~/docker/jellyfin/config
```

**Immich:**
```bash
# Backup library folder
rsync -av ~/docker/immich/library/ /backup/immich-library/
```

**Nextcloud:**
```bash
# Backup data folder (setelah set NEXTCLOUD_DATADIR)
rsync -av /mnt/nextcloud/ /backup/nextcloud/
```

**Portainer:**
```bash
# Backup Portainer data
docker run --rm -v portainer_data:/data -v $(pwd):/backup alpine \
  tar czf /backup/portainer-backup-$(date +%Y%m%d).tar.gz /data
```

### Monitoring Disk Space

```bash
# Cek disk usage
df -h

# Cek folder size
du -sh /path/to/folder

# Monitor real-time
watch -n 1 df -h
```

---

## 📝 Notes & Tips

### Best Practices

1. **Selalu backup sebelum update** - terutama Nextcloud dan Immich
2. **Monitor disk space** - media files bisa cepat penuh
3. **Gunakan SSD untuk config/cache** - lebih cepat
4. **Setup automated backups** - jangan sampe data hilang
5. **Keep containers updated** - security patches penting
6. **Document changes** - catat semua perubahan config

### Resources

- [Tailscale Docs](https://tailscale.com/kb/)
- [Docker Docs](https://docs.docker.com/)
- [Portainer Docs](https://docs.portainer.io/)
- [Beszel Docs](https://www.beszel.dev/guide/)
- [Jellyfin Docs](https://jellyfin.org/docs/)
- [Immich Docs](https://docs.immich.app/)
- [Nextcloud AIO GitHub](https://github.com/nextcloud/all-in-one)
- [Nextcloud AIO Tailscale Guide](https://github.com/nextcloud/all-in-one/discussions/5439)

---

## 🏷️ Tags

#ServerSetup #Debian #Tailscale #Docker #Portainer #Jellyfin #Immich #Nextcloud #SelfHosted #DevOps

---

**Last Updated:** {{date:YYYY-MM-DD}}
**OS:** Debian Bookworm/Bullseye
