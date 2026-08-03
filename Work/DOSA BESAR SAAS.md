1. denormalized di semua table
   >semua table harus pake 
   ```
   WHERE tenant_id = ?
   ```
   Ini keputusan arsitektur SaaS paling krusial — kebocoran data antar tenant biasanya terjadi karena ada satu query yang "lupa" filter tenant.
2. **Money `DECIMAL(15,2)`, metrik `BIGINT`.** Jangan pernah float/double buat uang.
3. **ID `BIGINT AUTO_INCREMENT`.** Simpel & cepat. Kalau lu paranoid ID bisa ditebak (enumeration), ganti ke UUIDv7 `BINARY(16)` — tapi keamanan antar tenant tetep dijaga tenant scoping, bukan obscurity ID.
4. **`rate_card` JSON** biar fleksibel per platform/jenis konten tanpa tabel tambahan. MySQL 8 bisa query JSON kalau nanti butuh.
5. **`platform`, `category`, `type` pakai VARCHAR, bukan ENUM** — biar nambah nilai baru cukup ubah validasi di app (go-playground/validator `oneof=...`), gak perlu migrasi. ENUM cuma gw pakai buat `status` yang alurnya emang kaku.
6. **`auth_tokens` nyimpen hash, bukan plaintext** — refresh token & reset token bisa di-revoke (logout beneran), dan kalau DB kebobolan token gak langsung kepake.
7. **Gak ada tabel agregat/summary di 1.0.** Dashboard & laporan dihitung on-the-fly dari `expenses` + `deliverables`. Kalau nanti data udah jutaan row dan query lambat, baru tambah summary table — jangan premature optimize.