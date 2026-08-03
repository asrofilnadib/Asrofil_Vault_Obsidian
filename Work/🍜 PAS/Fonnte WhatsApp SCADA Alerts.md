#Tech #Fonnte #WhatsApp #NewPAS #SCADA #Laravel #Supervisor #Notif

# Table of contents

- [[#Overview]]
- [[#Env Config (.env)]]
- [[#Token — yang sering bikin bingung]]
- [[#Device aktif]]
- [[#Architecture flow]]
- [[#Command checklist]]
- [[#Test API Fonnte (curl)]]
- [[#Supervisor]]
- [[#Dashboard Fonnte]]
- [[#Troubleshooting]]

## Overview

Fonnte dipakai buat **SCADA maintenance alerts** di NewPAS — kirim notifikasi WhatsApp ke grup engineering.


| Item       | Value                             |
| ---------- | --------------------------------- |
| Project    | `newpas-master`                   |
| Queue name | `notif-eng`                       |
| Target     | Grup WA `120363410394021831@g.us` |
| Provider   | [Fonnte](https://md.fonnte.com)   |
| API send   | `https://api.fonnte.com/send`     |


---



## Env Config (`.env`)

```env
FONNTE_TOKEN=xMUNm1Le......
FONNTE_API_URL=https://api.fonnte.com/send
FONNTE_COUNTRY_CODE=62
FONNTE_TARGETS=120363......@g.us
FONNTE_QUEUE=notif-eng
NOTIFICATIONS_ENABLED=true
```

> `FONNTE_TOKEN` di sini = **device token** (bukan account token). Lihat section Token di bawah.

---



## Token — yang sering bikin bingung

Fonnte punya **2 jenis token**. Jangan dituker.


| Jenis             | Dari mana                   | Dipakai buat                                         | Token sekarang            |
| ----------------- | --------------------------- | ---------------------------------------------------- | ------------------------- |
| **Device token**  | Device → tombol **Token**   | Kirim pesan (`/send`), cek status device (`/device`) | `xMUNm1LeYKMP8V4N9SFj`    |
| **Account token** | Setting → **Account Token** | Device API (`/get-devices`, manage device list)      | `wZgD2paCn4qXg13xpXHdasH` |


Contoh salah pakai:

```bash
# ❌ Device token di /get-devices → gagal
curl -X POST https://api.fonnte.com/get-devices \
  -H "Authorization: xMUNm1LeYKMP8V4N9SFj"
# → {"reason":"unknown user","status":false}

# ✅ Device token di /device → OK
curl -X POST https://api.fonnte.com/device \
  -H "Authorization: xMUNm1LeYKMP8V4N9SFj"
# → device_status: connect, quota, dll
```

Kalau butuh list semua device:

```bash
curl -X POST https://api.fonnte.com/get-devices \
  -H "Authorization: wZgD2paCn4qXg13xpXHdasH"
```

---



## Device aktif


| Name         | Nomor          | Status       | Package | Quota | Expired     |
| ------------ | -------------- | ------------ | ------- | ----- | ----------- |
| **Dani**     | `088980654429` | ✅ connect    | Free    | ~982  | 31 Jul 2026 |
| PAS DOWNTIME | `082323444038` | ❌ disconnect | Free    | 1000  | 31 Jul 2026 |


NewPAS pakai device **Dani**. Kalau status disconnect → reconnect dari dashboard, atau QR ulang.

Akun Fonnte:

- Email: `goldenpalker8@gmail.com`
- Country: Indonesia (+62)
- Timezone: `+7 Asia/Jakarta`
- Dashboard: [https://md.fonnte.com/new/device.php](https://md.fonnte.com/new/device.php)

---



## Architecture flow

```
SCADA event / notif trigger
        ↓
  notif_queue (DB)          ← dispatch status
        ↓
  Laravel queue: notif-eng
        ↓
  Supervisor workers        ← notif-dispatch + notif-queue-worker
        ↓
  Fonnte API /send
        ↓
  WhatsApp Group            ← 120363410394021831@g.us
```

---



## Command checklist



### 1. Cek status notifikasi (Laravel)

```bash
php artisan notif:status
```

Output yang diharapkan kira-kira:

```
=== notif_queue (DB test) ===
  sent         ...

=== 3 record terakhir ===
  #.. notif_id=.. status=sent ...

=== Laravel queue (notif-eng) ===
  jobs menunggu diproses: 0
  failed_jobs (semua queue): ...

=== Fonnte config ===
  [grup] 120363410394021831@g.us
```



### 2. Cek Supervisor

```bash
sudo supervisorctl status
```

Harus **RUNNING**:

```
notif-dispatch                             RUNNING
notif-queue-worker:notif-queue-worker_00   RUNNING
notif-queue-worker:notif-queue-worker_01   RUNNING
notif-queue-worker:notif-queue-worker_02   RUNNING
```

Restart kalau perlu:

```bash
sudo supervisorctl restart notif-dispatch
sudo supervisorctl restart notif-queue-worker:*
# atau semua:
sudo supervisorctl restart all
```



### 3. Cek failed jobs

```bash
php artisan queue:failed
# atau dari notif:status — lihat angka failed_jobs
```

Retry / flush (hati-hati di production):

```bash
php artisan queue:retry all
php artisan queue:flush   # hapus failed — jangan asal jalanin
```



### ⚠️ Jangan lakukan ini

**Jangan** jalankan `php artisan queue:work` / `queue:listen` manual **bersamaan** dengan Supervisor. Double worker = pesan dobel / race condition.

---



## Test API Fonnte (curl)



### Cek device (pakai device token)

```bash
curl -X POST https://api.fonnte.com/device \
  -H "Authorization: xMUNm1LeYKMP8V4N9SFj"
```

Response sehat:

```json
{
  "status": true,
  "device": "088980654429",
  "device_status": "connect",
  "name": "Dani",
  "package": "Free",
  "quota": "982",
  "expired": "31 July 2026"
}
```



### List devices (pakai account token)

```bash
curl -X POST https://api.fonnte.com/get-devices \
  -H "Authorization: wZgD2paCn4qXg13xpXHdasH"
```



### Kirim pesan test ke grup

```bash
curl -X POST https://api.fonnte.com/send \
  -H "Authorization: xMUNm1LeYKMP8V4N9SFj" \
  -d "target=120363410394021831@g.us" \
  -d "message=Test alert dari NewPAS — ignore"
```

> Tiap kirim makan quota. Free package terbatas — jangan spam test.

---



## Supervisor


| Program                   | Fungsi                                |
| ------------------------- | ------------------------------------- |
| `notif-dispatch`          | Dispatch job notifikasi ke queue      |
| `notif-queue-worker` (x3) | Proses queue `notif-eng` → hit Fonnte |


Config biasanya di `/etc/supervisor/conf.d/` — cek file `notif-*.conf` kalau worker mati terus.

---



## Dashboard Fonnte


| Halaman         | URL                                                                            | Buat apa                                |
| --------------- | ------------------------------------------------------------------------------ | --------------------------------------- |
| Device          | [https://md.fonnte.com/new/device.php](https://md.fonnte.com/new/device.php)   | Status connect, Token device, Reconnect |
| Setting         | [https://md.fonnte.com/new/setting.php](https://md.fonnte.com/new/setting.php) | Account token, timezone, email          |
| Send            | sidebar **Send**                                                               | Test manual tanpa curl                  |
| Message History | sidebar                                                                        | Log pesan terkirim / gagal              |


---



## Troubleshooting


| Gejala                      | Cek                                  | Fix                                                 |
| --------------------------- | ------------------------------------ | --------------------------------------------------- |
| `unknown user` di API       | Token salah jenis                    | Device token vs account token — lihat tabel di atas |
| `device_status: disconnect` | Dashboard Device                     | Klik **Reconnect**, scan QR                         |
| `jobs menunggu` numpuk      | `supervisorctl status`               | Restart worker yang STOPPED                         |
| `failed_jobs` naik          | `queue:failed` + log Laravel         | Cek token, target grup, quota                       |
| Notif mati total            | `.env` `NOTIFICATIONS_ENABLED`       | Pastikan `true`, lalu `php artisan config:clear`    |
| Quota 0                     | Dashboard Device                     | Order paket / tunggu reset (cek policy Free)        |
| Pesan dobel                 | Ada `queue:work` manual + Supervisor | Matikan proses manual                               |


Quick health check (urutan):

```bash
# 1. Worker hidup?
sudo supervisorctl status

# 2. Queue & DB notif OK?
php artisan notif:status

# 3. Fonnte device connect?
curl -X POST https://api.fonnte.com/device \
  -H "Authorization: xMUNm1LeYKMP8V4N9SFj"
```

Kalau ketiganya hijau → pipeline notif sehat.