#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kenapa Pakai Transaction?|Kenapa Pakai Transaction?]]
- [[#Implementasi|Implementasi]]
	- [[#Implementasi#Penerapan didalam Try Catch|Penerapan didalam Try Catch]]

## Introduction
Ada case dimana gue melakukan 3 operasi sekaligus dalam 1 waktu (1 method). Contoh ketika melakukan pemesanan material otomatis table material, table stok hingga table storloc akan terupdate. Kalau salah satu tidak terupdate maka data tidak presisi. Banyak keluhan (kok stok tidak sesuai?). DB Transaction adalah jawaban nya!

## Kenapa Pakai Transaction?
**Database transaction** digunakan untuk memastikan bahwa **sekelompok operasi database dijalankan secara utuh (semua berhasil) atau tidak dijalankan sama sekali (jika ada yang gagal)**. Ini penting untuk menjaga **konsistensi dan integritas data** dalam aplikasi kamu.

Contoh pentingnya:

Bayangkan kamu sedang membuat fitur transfer uang antar pengguna:
1. Kurangi saldo pengguna A.
2. Tambahkan saldo ke pengguna B.

## Implementasi
```php
use Illuminate\Support\Facades\DB;

DB::transaction(function () {
    $userA = User::find(1);
    $userB = User::find(2);

    $userA->balance -= 100;
    $userA->save();

    $userB->balance += 100;
    $userB->save();
});
```

### Penerapan didalam Try Catch
```php
use Illuminate\Support\Facades\DB;
use Illuminate\Http\Request;
use Exception;

try {
    DB::transaction(function () {
        $userA = User::find(1);
        $userB = User::find(2);

        if (!$userA || !$userB) {
            throw new Exception('User tidak ditemukan');
        }

        $userA->balance -= 100;
        $userA->save();

        $userB->balance += 100;
        $userB->save();
    });

    return response()->json(['message' => 'Transfer berhasil!']);

} catch (Exception $e) {
    return response()->json(['error' => 'Transfer gagal: ' . $e->getMessage()], 500);
}
```

Date: 16-10-2025