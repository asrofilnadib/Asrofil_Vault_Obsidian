### cara untuk kirim file dari computer lokal ke vps
```bash
# Upload file SQL
scp ~/Downloads/pas-dump.sql root@62.72.44.86:/root/

# Upload folder project
rsync -avz -e ssh ~/project-lu/ root@62.72.44.86:/var/www/portal-pas/

# Restore database di VPS
ssh root@62.72.44.86
mariadb -u root -p portal_pas < /root/pas-dump.sql
```

### how to connect remote fresh mysql on debian
#### 2. Cek Bind Address MariaDB

MariaDB default cuma denger di `127.0.0.1`. Cek config:
```bash
nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
Cari baris `bind-address`, ganti jadi:
```bash
bind-address = 0.0.0.0
# Atau spesifik IP VPS lu:
# bind-address = 100.93.119.87
```
Kalo gak ada, tambahin di bawah `[mysqld]`.

