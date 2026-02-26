#Tech 
## Introduction
Lanjutan dari jurnal [[Clean Code Function Pakai Traits]], Ada case dimana gue sering banget ngulang ngulang code yg buat gue cape setengah mati. Karena project udah kompleks. Case nya gue pakai telegram.
## Apa Itu Services?
Services adalah class yang berisi logika bisnis aplikasi yang dikemas secara terpisah dari controller dan model untuk menjaga kode tetap terorganisir, reusable, dan mudah di-maintain.
## Telegram Services
App\Services\NamaAplikasi\Telegram
```php
<?php

namespace App\Services\TMS;

use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Log;
use Illuminate\Support\Facades\Http;

class TelegramService
{
    public function sendToAndaru($text)
    {
        $apiUrl = "https://api.telegram.org/bot{token}/sendMessage";
        $chat_id = 'xxx';
        $response = Http::post($apiUrl, [
            'chat_id' => $chat_id,
            'text' => "GAGAL -> ".$text,
            'parse_mode' => 'HTML'
        ]);

        return [
            'status' => $response->successful() ? 'Berhasil kirim pesan Gagal' : 'Gagal'
        ];
    }

    public function sendMessage($dept, $text)
    {
        $telegram = DB::table('tms_master_telegram')->where('dept', $dept)->first();

        if($telegram){
            $token = $telegram->token;
            $chat_id = $telegram->chat_id;
    
            $apiUrl = "https://api.telegram.org/bot".$token."/sendMessage";
    
            $response = Http::post($apiUrl, [
                'chat_id' => $chat_id,
                'text' => $text,
                'parse_mode' => 'HTML'
            ]);
    
            return [
                'status' => $response->successful() ? 'Berhasil kirim pesan' : 'Gagal',
                'body' => $response->json()
            ];
        }else{
            $this->sendToAndaru($text);
        }
    }

    public function sendPhoto($dept, $photoPath, $caption = '')
    {
        $telegram = DB::table('tms_master_telegram')->where('dept', $dept)->first();
        
        if($telegram){
            $token = $telegram->token;
            $chat_id = $telegram->chat_id;
            $url = "https://api.telegram.org/bot{$token}/sendPhoto";

            $mime = mime_content_type($photoPath);
            
            $allowedMimes = ['image/jpeg', 'image/jpg', 'image/png', 'image/webp'];
            if (!in_array($mime, $allowedMimes)) {
                Log::warning("File bukan gambar: " . $mime);
                return false;
            }

            $filename = basename($photoPath);
            $cFile = new \CURLFile($photoPath, $mime, $filename);

            $postFields = [
                'chat_id' => $chat_id,
                'photo' => $cFile,
                'caption' => $caption,
                'parse_mode' => 'Markdown'
            ];

            $ch = curl_init();
            curl_setopt_array($ch, [
                CURLOPT_URL => $url,
                CURLOPT_POST => true,
                CURLOPT_POSTFIELDS => $postFields,
                CURLOPT_HTTPHEADER => ["Content-Type: multipart/form-data"],
                CURLOPT_RETURNTRANSFER => true,
                CURLOPT_SSL_VERIFYPEER => false, 
            ]);

            $result = curl_exec($ch);

            if (curl_errno($ch)) {
                Log::error("Telegram Send Photo Error: " . curl_error($ch));
            }

            curl_close($ch);

            return $result;
        }else{
            $this->sendToAndaru();
        }
    }
}
```
Penjelasan:
Di class ini gue membuat 3 method dimana untuk pengiriman pesan ke telegram, pengiriman photo ke telegram dan pengiriman pesan error ke telegram pribadi
## Implementasi ke Controller
```php
protected $telegram;

public function __construct(TelegramService $telegram)
{
	$this->telegram = $telegram;
}

public function contoh(){
	$text = "Haloo";
	$this->telegram->sendMessage('ALL', $text);
}
```

Kalau lo bingung dapat token dan chat id nya dimana? Bisa lihat jurnal [[Membuat Telegram BOT]]

Note: Ini tidak perlu install telegram third party laravel. Karena kita langsung tembak ke endpoint nya.

Date: 28-07-2025