---
tags:
  - kol/master
alias: Master Plan
status: planning
phase: 1
created: 2026-08-04
target_mvp: 2026-10-30
stack: go, react-vite, mysql
---

# 🎯 CampaignHub KOL — Master Plan v1.0

> [!abstract] North Star
> SaaS manajemen campaign KOL: **KOL, budget, dan output dalam satu tempat**.
> Inti produk = tracking **Planned vs Realisasi + Output**.
> Ujungnya: **data siap saji buat BOD** (budget vs real, reach, engagement, CPE).

## 🧭 Business Flow (v1.0)

1. **Onboarding** — daftar tenant (org) → login
2. **Aset KOL** — input database KOL + rate card
3. **Planning** — campaign → budget & periode → pilih KOL + fee nego + rencana deliverable
4. **Eksekusi** — catat pengeluaran real + catat output (link post & metrik)
5. **Monitoring** — dashboard burn rate, deadline, campaign aktif
6. **Reporting** — rekap BOD → ekspor Excel/CSV

## 📦 Scope v1.0 (Core & Sub-Feature)

| # | Core | Sub-feature |
|---|------|-------------|
| 1 | Akses Akun | daftar tenant, login/logout, forgot password, role owner/member |
| 2 | Database KOL | list + search/filter, tambah & edit, detail + riwayat campaign |
| 3 | Manajemen Campaign | buat/edit (budget & periode), atur KOL + fee nego, plan deliverable per KOL |
| 4 | Tracking Output | input link post + metrik, status deliverable |
| 5 | Pencatatan Pengeluaran | form pengeluaran + bukti bayar, riwayat + filter |
| 6 | Dashboard | progress budget, notif deadline, campaign aktif |
| 7 | Laporan & Rekap | budget vs real + output summary, ekspor Excel/CSV |
| 8 | Pengaturan Profil | info akun, ganti password, preferensi notif |

> [!warning]- ✂️ DIPOTONG dari v1.0 (jangan goda diri sendiri)
> - Billing (Stripe/Midtrans)
> - Auto-fetch metrik IG/TikTok (input manual dulu)
> - Approval flow & audit log
> - Role granular (owner/member cukup)
> - Notif WhatsApp (email dulu)
> - Multi-currency (IDR only)

## 🗄️ Data Model (ringkas)

> [!note]- Skema — klik untuk expand
> - `tenants` — org/brand (SaaS tenant)
> - `users` — tenant_id, role owner/member
> - `auth_tokens` — refresh & password_reset, **simpan hash only**
> - `kols` — rate_card JSON, FULLTEXT(name, username)
> - `campaigns` — budget_total DECIMAL(15,2), status draft/active/completed/cancelled
> - `campaign_kols` — N:M campaign↔KOL, `negotiated_fee` = komitmen biaya
> - `deliverables` — nempel ke campaign_kol; metrik views/likes/comments/shares
> - `expenses` — kategori, status planned/paid, due_date = tempo bayar
> - `notifications` — diisi cron scan deadline
>
> 📎 DDL lengkap: [[CampaignHub - DDL & Schema]]

> [!danger] Guardrails teknis — baca sebelum nulis query
> - **`tenant_id` di SEMUA tabel**, semua query list wajib filter tenant. Kebocoran data antar tenant = bug #1 SaaS multi-tenant.
> - Uang = `DECIMAL(15,2)`. Jangan pernah float.
> - **Jangan double-count**: planned = SUM(negotiated_fee) + expenses non-`kol_fee`; real = SUM(expenses `paid`).
> - Refresh token di httpOnly cookie, disimpan sebagai hash di DB.
> - `platform/category/type` = VARCHAR + validasi app; ENUM hanya untuk status.
> - Jangan bikin tabel agregat dulu; hitung on-the-fly sampai terbukti lambat.

## 🛠️ Tech Stack

| Layer | Pilihan |
|---|---|
| BE router | chi v5 |
| BE DB | sqlc + go-sql-driver/mysql, migrasi goose |
| BE auth | golang-jwt v5 + argon2id |
| BE misc | validator v10, caarlos0/env, slog, robfig/cron, excelize, swaggo |
| FE core | React + Vite + TS, Tailwind + shadcn/ui |
| FE data | TanStack Query v5, TanStack Table, React Hook Form + zod |
| FE ui | Recharts, react-day-picker, sonner, dayjs, zustand, axios |

## 🗺️ Roadmap & Exit Criteria

| Phase | Focus | Window | Exit criteria |
|---|---|---|---|
| P0 | Foundation & Auth | 04/08–21/08 | register→login jalan, tenant scoping aktif |
| P1 | Database KOL | 24/08–04/09 | CRUD KOL + search + detail riwayat |
| P2 | Campaign | 07/09–18/09 | campaign + fee nego + deliverable plan (transactional) |
| P3 | Expenses & Output | 21/09–02/10 | pencatatan real + output + cron notif |
| P4 | Dashboard | 05/10–16/10 | burn rate, deadline, campaign aktif live |
| P5 | Laporan & Hardening | 19/10–30/10 | ekspor Excel, swagger, staging E2E ✅ |

## ✅ Things-to-do

### P0 — Foundation
- [ ] Init monorepo: `/api` (Go chi) + `/apps/web` (Vite React TS) 🔺 📅 2026-08-05 #kol/p0
- [ ] docker-compose: MySQL 8 + Mailhog (+ MinIO opsional) 🔺 📅 2026-08-06 #kol/p0
- [ ] Goose migration `0001_init` dari [[CampaignHub - DDL & Schema]] 🔺 📅 2026-08-07 #kol/p0
- [ ] sqlc setup + generate query pertama (users) ⏫ 📅 2026-08-10 #kol/p0
- [ ] BE auth: register tenant+user, login, JWT access + refresh httpOnly, argon2id 🔺 📅 2026-08-14 #kol/p0
- [ ] Middleware tenant scoping — repo wajib terima tenantID 🔺 📅 2026-08-14 #kol/p0
- [ ] FE auth: form login/register (RHF+zod), axios interceptor, protected route 🔺 📅 2026-08-21 #kol/p0

### P1 — Database KOL
- [ ] BE: CRUD KOL + pagination + filter + FULLTEXT search ⏫ 📅 2026-08-26 #kol/p1
- [ ] FE: tabel KOL (TanStack Table, server-side) ⏫ 📅 2026-08-28 #kol/p1
- [ ] FE: form KOL dengan rate_card dinamis per platform ⏫ 📅 2026-09-01 #kol/p1
- [ ] FE: detail KOL + riwayat campaign ⏫ 📅 2026-09-04 #kol/p1

### P2 — Campaign
- [ ] BE: create campaign transactional (campaign + campaign_kols + deliverables plan) 🔺 📅 2026-09-09 #kol/p2
- [ ] FE: wizard campaign (info → budget → KOL+fee → deliverable plan) 🔺  2026-09-11 #kol/p2
- [ ] FE: range picker tanggal (react-day-picker) 🔼 📅 2026-09-11 #kol/p2
- [ ] BE+FE: list & detail campaign (status, progress komitmen vs budget) ⏫ 📅 2026-09-16 #kol/p2

### P3 — Expenses & Output
- [ ] BE: CRUD expenses + upload bukti (presigned URL) ⏫ 📅 2026-09-23 #kol/p3
- [ ] FE: form pengeluaran (money input Intl.NumberFormat) + riwayat filter ⏫  2026-09-25 #kol/p3
- [ ] FE: input output/deliverable — link + metrik, auto-detect platform ⏫  2026-09-30 #kol/p3
- [ ] BE: cron (robfig/cron) — sweep overdue + notif H-3/H-1 deadline ⏫ 📅 2026-10-02 #kol/p3

### P4 — Dashboard
- [ ] BE: endpoint `/dashboard/summary` (agregat) ⏫ 📅 2026-10-07 #kol/p4
- [ ] FE: kartu campaign aktif + progress bar burn rate ⏫ 📅 2026-10-09 #kol/p4
- [ ] FE: Recharts — burn rate line + budget vs real bar ⏫ 📅 2026-10-12 #kol/p4
- [ ] FE: popover notifikasi (polling) 🔼 📅 2026-10-14 #kol/p4

### P5 — Laporan & Hardening
- [ ] BE: agregat laporan (budget vs real + reach/engagement/CPE) 🔺 📅 2026-10-21 #kol/p5
- [ ] BE: ekspor Excel (excelize) + CSV 🔺  2026-10-23 #kol/p5
- [ ] FE: halaman laporan + download blob ⏫ 📅 2026-10-26 #kol/p5
- [ ] Swagger (swaggo) + README setup ⏫ 📅 2026-10-28 #kol/p5
- [ ] Deploy staging + test E2E flow penuh 🔺 📅 2026-10-30 #kol/p5

### 🔁 Ritual
- [ ] Weekly review: update progress, geser due date, catat keputusan di log 🔼 📅 2026-08-07 🔄 every week on Friday #kol/ritual

### ❓ Keputusan bisnis yang belum diambil
- [ ] Target user: brand in-house atau agency? (ngaruh ke copy & onboarding) 📅 2026-08-14 #kol/biz
- [ ] Pricing model: per seat atau per campaign? 📅 2026-08-28 #kol/biz
- [ ] Laporan BOD: Excel cukup atau perlu PDF branded? 📅 2026-10-05 #kol/biz

## 📊 Dashboard Project

```dataviewjs
const all = dv.pages().file.tasks.flatten();
const t = all.filter(x => x.text.includes("#kol"));
const done = t.filter(x => x.completed).length;
const pend = t.filter(x => !x.completed).length;
dv.table(["Metric", "Count"], [
  ["✅ Completed", done],
  ["🔄 Pending", pend],
  ["Progress", Math.round(done / (done + pend) * 100) + "%"]
]);
```

### 🚨 Overdue
```tasks
not done
tags include #kol
due before today
sort by priority
```

### 🎯 Due Today
```tasks
not done
tags include #kol
due today
```

### 📅 Due This Week
```tasks
not done
tags include #kol
due this week
sort by due
```

### 🏁 Done This Week
```tasks
done this week
tags include #kol
```

##  Decision Log (ADR mini)

> [!quote] ADR-001 — Multi-tenancy
> Single DB, kolom `tenant_id` di semua tabel, enforcement di repository layer. Paling simpel, cukup aman untuk skala awal.

> [!quote] ADR-002 — DB access
> sqlc, bukan GORM. SQL type-safe, query terkontrol, cocok buat query agregat laporan.

> [!quote] ADR-003 — Auth
> JWT access short-lived + refresh token httpOnly cookie (hash di DB, bisa revoke).

> [!quote] ADR-004 — Metrik output
> Input manual di v1.0. Auto-fetch API Meta/TikTok ditunda (approval ribet).

> [!quote] ADR-005 — Uang
> IDR only, `DECIMAL(15,2)`.

## ️ Scratchpad

<!-- area bebas buat coretan ide random, biar gak nyelip di section lain -->
-