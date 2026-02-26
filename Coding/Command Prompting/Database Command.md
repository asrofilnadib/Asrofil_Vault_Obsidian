### export dump sql ke mysql
```bash
mysqldump -u root -p database_A > /home/asrofil/dump_database_A.sql
```

### inject dump sql ke ubuntu mysql
```bash
mysql -u root -p database_B < /home/asrofil/dump_database_A.sql
```

### Grant user for spesific table
> buat user dulu
```bash
CREATE USER 'asrofil'@'%' IDENTIFIED BY "password ni.."
```
> READ only ke semua table (BASELINE)
```bash
GRANT SELECT
ON kocak_2.*
TO 'app_user'@'%';
```
> AUTO-GENERATE FULL ACCESS untuk spesifik table, contoh:`tms_*` & `smart_lab_*`
```bash
SELECT CONCAT(
  'GRANT INSERT, UPDATE, DELETE ON kocak_2.', table_name,
  ' TO ''app_user''@''%'';'
) AS grant_sql
FROM information_schema.tables
WHERE table_schema = 'kocak_2'
  AND (
    table_name LIKE 'tms_%'
    OR table_name LIKE 'smart_lab_%'
  );
```
> Apply
```bash
FLUSH PRIVILEGES;
```
> Cek hasilnya coyy
```bash
SHOW GRANTS FOR 'app_user'@'%';
```

## Problems
1. kalau export mysqldump dari product jetbrains jangan gunakan lock tables. Fungsi dari lock tables itu seperti "freeze" sementara db, jadinya ketika ada transaksi baru di table manapun maka gabisa insert tuh... datanya, dimaksudkan sih biar ga saling tumpang tindih... biar ga nabrak 