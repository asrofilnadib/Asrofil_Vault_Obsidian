#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Install Packages|Install Packages]]
- [[#Config App|Config App]]
- [[#Publish Vendornya|Publish Vendornya]]
- [[#Panggil Classnya|Panggil Classnya]]
- [[#Buat Bot Telegram|Buat Bot Telegram]]
	- [[#Buat Bot Telegram#1. BotFather|1. BotFather]]
	- [[#Buat Bot Telegram#**2. ID Bot**|**2. ID Bot**]]
- [[#.env|.env]]

## Introduction
Saya mencoba bot telegram ketika ada interview Magang di PT. Prakarsa Alam Segar atau Wings Group.

## Install Packages
Kita akan membutuhkan package sdk telegram.

```
composer require irazasyed/telegram-bot-sdk
```

## Config App
Selanjutnya buka folder config/app.php dan masukkan provider dan aliases dibawah :

```php
'providers' => ServiceProvider::defaultProviders()->merge([
	...
	Telegram\Bot\Laravel\TelegramServiceProvider::class,
])->toArray(),

'aliases' => Facade::defaultAliases()->merge([
	...
	'Telegram' => Telegram\Bot\Laravel\Facades\Telegram::class,
])->toArray(),
```

## Publish Vendornya

```bash
php artisan vendor:publish --provider="Telegra m\Bot\Laravel\TelegramServiceProvider"
```

## Panggil Classnya
```php
use Telegram\Bot\Laravel\Facades\Telegram;
```

dan contoh implementasi ke Controller :

```php
public function store(Request $req)
{
	// Logic Aplikasi
	$this->sendTelegramMessage("Pelanggan baru telah ditambahkan. Selamat datang {$req->name}");
	return response()->json(['success' => true]);
}
```

Berikut method sendTelegramMessagenya :

```php
private function sendTelegramMessage($message)
{
	$token = env('TELEGRAM_BOT_TOKEN');
	$chatId = env('TELEGRAM_GROUP_CHAT_ID');

	Telegram::sendMessage([
		'chat_id' => $chatId,
		'text' => $message
	]);
}
```

## Buat Bot Telegram
Kita akan membutuhkan 2 Bot
1. BotFather : bot kita. Kita butuh Token dan Username nya.
2. ID Bot : untuk forward message di group atau channel. Kita butuh Chat ID.

### 1. BotFather
- Klik cari, dan ketik @botfather.
- Setelah itu ketik `/start`.
- `/newbot` untuk membuat bot baru. Kamu akan disuruh memasukkan name dan username
- Setelah itu akan diberitahukan token kita.

![[Bot Father.png]]

### **2. ID Bot**
- Cari @idbot
- Ketik `/start`
- dan Terakhir untuk mendapatkan chat id kita harus undang ID Bot ke group. dan secara otomatis akan diberikan chat id nya.

![[ID BOT.png]]

Jangan lupa untuk load ke `.env`.

Update: Chat ID itu seperti penanda suatu group atau user. Tanpa chat id kita gabisa kirim pesan. Cara tahu chat id nya bisa langsung ke IDBOT.

![[id bot baru tele 2025.png]]
## .env
Tambahkan kode berikut :

```
TELEGRAM_BOT_USERNAME=andarutr_bot
TELEGRAM_BOT_TOKEN=7025576016:AAETAAsYi0bvoiwVE651gqVwWzPJpXjc4YY
TELEGRAM_GROUP_CHAT_ID=-4111179476
```

Referensi : [https://egin10.medium.com/membuat-bot-telegram-dengan-laravel-8-f9fce9b2bb56](https://egin10.medium.com/membuat-bot-telegram-dengan-laravel-8-f9fce9b2bb56)

Date : 19-02-2024