#Tech #Git
# Topics
- [[#Introduction]]
- [[#Langkah Konfigurasi]]
- [[#Penjelasan singkat]]

## Introduction
membuat default push dan default pull ke remote yang berbeda, ini bentuk dari efisiensi kinerja.
- **Push** → selalu ke `nadib/dev` (repo pribadimu)
    
- **Pull** → tetap ambil dari `origin/master` (repo utama PT PAS)

## Langkah Konfigurasi

Pastikan kamu sekarang lagi di branch `dev`:
```bash
git checkout dev
```
kode diatas untuk berpindah antar branch

Lalu jalankan perintah berikut:
```bash
# Set upstream (pull) ke origin/master
git branch --set-upstream-to=origin/master dev

# Set default push ke nadib/dev
git config branch.dev.pushRemote nadib
```

## Penjelasan singkat

- `branch.dev.pushRemote nadib` → artinya kalau kamu `git push`, Git akan mengirim ke remote `nadib` (bukan origin).
- `--set-upstream-to=origin/master` → artinya kalau kamu `git pull`, Git akan mengambil update dari `origin/master`.

Jadi alurnya nanti begini:
```bash
git pull    # ambil update dari PT-PAS (origin/master)
git push    # kirim perubahan ke repo pribadimu (nadib/dev)
```

## Cek hasil konfigurasi

Untuk memastikan semuanya sudah benar:
```bash
git remote -v git config --get branch.dev.remote 
git config --get branch.dev.pushRemote git status
```
Hasil yang diharapkan kira-kira seperti ini:
```bash
branch.dev.remote=origin 
branch.dev.pushRemote=nadib
```