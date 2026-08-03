```mysql
-- ============ TENANCY & AUTH ============
CREATE TABLE tenants (
  id         BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  name       VARCHAR(100) NOT NULL,               -- nama org/brand
  plan       VARCHAR(20)  NOT NULL DEFAULT 'free', -- siap buat billing nanti
  status     ENUM('active','suspended') NOT NULL DEFAULT 'active',
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE users (
  id            BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  tenant_id     BIGINT UNSIGNED NOT NULL,
  name          VARCHAR(100) NOT NULL,
  email         VARCHAR(255) NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  role          ENUM('owner','member') NOT NULL DEFAULT 'member',
  status        ENUM('active','disabled') NOT NULL DEFAULT 'active',
  created_at    DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at    DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  UNIQUE KEY uq_users_email (email),             -- login pakai email, global unique
  KEY idx_users_tenant (tenant_id),
  CONSTRAINT fk_users_tenant FOREIGN KEY (tenant_id) REFERENCES tenants (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE auth_tokens (
  id         BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  user_id    BIGINT UNSIGNED NOT NULL,
  tenant_id  BIGINT UNSIGNED NOT NULL,
  type       ENUM('refresh','password_reset') NOT NULL,
  token_hash CHAR(64) NOT NULL,                  -- simpan SHA-256, jangan plaintext
  expires_at DATETIME NOT NULL,
  revoked_at DATETIME NULL,                      -- isi saat logout/revoke
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  UNIQUE KEY uq_auth_tokens_hash (token_hash),
  KEY idx_auth_tokens_user (user_id),
  CONSTRAINT fk_auth_tokens_user FOREIGN KEY (user_id) REFERENCES users (id) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ============ KOL ============
CREATE TABLE kols (
  id            BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  tenant_id     BIGINT UNSIGNED NOT NULL,
  name          VARCHAR(150) NOT NULL,
  platform      VARCHAR(20)  NOT NULL,           -- instagram/tiktok/youtube, validasi di app
  username      VARCHAR(150) NOT NULL,
  profile_url   VARCHAR(255) NULL,
  followers     BIGINT UNSIGNED NOT NULL DEFAULT 0,
  engagement_rate DECIMAL(5,2) NULL,             -- persen
  category      VARCHAR(50)  NULL,               -- beauty, tech, food...
  rate_card     JSON         NULL,               -- {"ig_post":750000,"ig_reel":1200000,...}
  contact_phone VARCHAR(30)  NULL,
  notes         TEXT         NULL,
  status        ENUM('active','archived') NOT NULL DEFAULT 'active',
  created_at    DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at    DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  KEY idx_kols_tenant_status (tenant_id, status),
  FULLTEXT KEY ft_kols_search (name, username),
  CONSTRAINT fk_kols_tenant FOREIGN KEY (tenant_id) REFERENCES tenants (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ============ CAMPAIGN ============
CREATE TABLE campaigns (
  id           BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  tenant_id    BIGINT UNSIGNED NOT NULL,
  name         VARCHAR(150) NOT NULL,
  description  TEXT NULL,
  start_date   DATE NOT NULL,
  end_date     DATE NOT NULL,
  budget_total DECIMAL(15,2) NOT NULL DEFAULT 0, -- budget planned
  status       ENUM('draft','active','completed','cancelled') NOT NULL DEFAULT 'draft',
  created_by   BIGINT UNSIGNED NOT NULL,
  created_at   DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at   DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  KEY idx_campaigns_tenant_status (tenant_id, status),   -- dashboard: campaign aktif
  KEY idx_campaigns_tenant_end (tenant_id, end_date),    -- notif campaign mau selesai
  CONSTRAINT fk_campaigns_tenant FOREIGN KEY (tenant_id) REFERENCES tenants (id),
  CONSTRAINT fk_campaigns_creator  FOREIGN KEY (created_by) REFERENCES users (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE campaign_kols (
  id             BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  tenant_id      BIGINT UNSIGNED NOT NULL,
  campaign_id    BIGINT UNSIGNED NOT NULL,
  kol_id         BIGINT UNSIGNED NOT NULL,
  negotiated_fee DECIMAL(15,2) NOT NULL DEFAULT 0, -- komitmen biaya KOL
  notes          VARCHAR(255) NULL,
  created_at     DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  UNIQUE KEY uq_ck_campaign_kol (campaign_id, kol_id),   -- 1 KOL 1x per campaign
  KEY idx_ck_tenant_kol (tenant_id, kol_id),             -- riwayat campaign di detail KOL
  CONSTRAINT fk_ck_campaign FOREIGN KEY (campaign_id) REFERENCES campaigns (id) ON DELETE CASCADE,
  CONSTRAINT fk_ck_kol      FOREIGN KEY (kol_id)      REFERENCES kols (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

CREATE TABLE deliverables (
  id              BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  tenant_id       BIGINT UNSIGNED NOT NULL,
  campaign_kol_id BIGINT UNSIGNED NOT NULL,
  type            VARCHAR(30) NOT NULL,          -- ig_post/ig_reel/tiktok_video/yt_shorts
  due_date        DATE NOT NULL,
  status          ENUM('planned','published','overdue','cancelled') NOT NULL DEFAULT 'planned',
  post_url        VARCHAR(255) NULL,             -- isi saat published
  published_at    DATETIME NULL,
  views           BIGINT UNSIGNED NOT NULL DEFAULT 0,
  likes           BIGINT UNSIGNED NOT NULL DEFAULT 0,
  comments        BIGINT UNSIGNED NOT NULL DEFAULT 0,
  shares          BIGINT UNSIGNED NOT NULL DEFAULT 0,
  created_at      DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at      DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  KEY idx_deliv_ck (campaign_kol_id),
  KEY idx_deliv_deadline (status, due_date),     -- cron scan: planned lewat deadline -> overdue
  CONSTRAINT fk_deliv_ck FOREIGN KEY (campaign_kol_id) REFERENCES campaign_kols (id) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ============ PENGELUARAN ============
CREATE TABLE expenses (
  id          BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  tenant_id   BIGINT UNSIGNED NOT NULL,
  campaign_id BIGINT UNSIGNED NOT NULL,
  category    VARCHAR(30) NOT NULL,              -- kol_fee/production/ads/transport/other
  title       VARCHAR(150) NOT NULL,             -- "Fee @username - IG Reel"
  amount      DECIMAL(15,2) NOT NULL,
  status      ENUM('planned','paid','cancelled') NOT NULL DEFAULT 'planned',
  due_date    DATE NULL,                         -- tempo bayar -> notif deadline
  paid_at     DATETIME NULL,
  receipt_url VARCHAR(255) NULL,                 -- bukti bayar (upload)
  notes       TEXT NULL,
  created_by  BIGINT UNSIGNED NOT NULL,
  created_at  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  KEY idx_exp_campaign (tenant_id, campaign_id, status), -- burn rate per campaign
  KEY idx_exp_deadline (status, due_date),               -- cron scan tempo bayar
  CONSTRAINT fk_exp_campaign FOREIGN KEY (campaign_id) REFERENCES campaigns (id),
  CONSTRAINT fk_exp_creator  FOREIGN KEY (created_by)  REFERENCES users (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ============ NOTIFIKASI ============
CREATE TABLE notifications (
  id         BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  tenant_id  BIGINT UNSIGNED NOT NULL,
  type       VARCHAR(30) NOT NULL,               -- deliverable_deadline/payment_deadline/campaign_ending
  message    VARCHAR(255) NOT NULL,
  ref_type   VARCHAR(20) NULL,                   -- 'deliverable' | 'expense' | 'campaign'
  ref_id     BIGINT UNSIGNED NULL,
  due_date   DATE NULL,
  read_at    DATETIME NULL,
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  KEY idx_notif_tenant_read (tenant_id, read_at),
  CONSTRAINT fk_notif_tenant FOREIGN KEY (tenant_id) REFERENCES tenants (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```