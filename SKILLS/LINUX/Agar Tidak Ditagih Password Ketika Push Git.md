![[Screenshot from 2025-04-18 17-40-16.png]]
#Tech #Linux 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Credential Store|Credential Store]]

## Introduction
Ada case dimana ketika lo pake Linux, dan lo mau push ke repo pribadi lo malah ditagih password (token) github lo. Akan sangat risih ketika lo sering push git. Berikut Solusinya

## Credential Store

```bash
sudo git config --global credential.helper store
```

Dengan mengetik perintah diatas maka lo cukup input password (token) lo ketika push repo. Kedepannya ngga akan ditagih lagi.

## Error
![[Pasted image 20250419072222.png]]

Jika mengalami error seperti ini coba buatkan token classic Github lo. lalu ketik perintah `sudo git config --global credential.helper store`

lalu git pull atau push

Untuk memastikan apakah credentials sudah terinput atau belum silahkan ketik perintah 

```bash
sudo chmod 777 ~/.git-credentials
nano ~/.git-credentials
```

chmod 777 itu mengubah permission akses menjadi All User.

Maka file tersebut akan berisikan

```
https://<username_github>:ghp_<tokenclassic>@github.com
```

Date: 18-04-2025