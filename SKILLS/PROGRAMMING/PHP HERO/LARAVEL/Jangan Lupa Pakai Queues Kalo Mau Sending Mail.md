#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Queue : work|Queue : work]]
- [[#Queue : listen|Queue : listen]]
- [[#Studi Kasus|Studi Kasus]]

## Introduction
Saya sedang mengerjakan project yg ada keperluan ngirim email ke department HR dan GA. Namun memiliki kendala ketika di pengiriman email yg agak lambat.

## Queue : work
Digunakan untuk menjalankan pekerjaan dalam _antrian_ secara _manual_. Ini berarti bahwa Anda harus _menjalankan perintah ini_ secara eksplisit _setiap kali ingin memproses antrian_. Proses ini akan menjalankan pekerjaan yang ada dalam antrian dan kemudian mengakhiri prosesnya. Dengan kata lain, setelah semua pekerjaan dalam antrian diproses, proses tersebut akan berhenti.

## Queue : listen
Menangkap antrian secara _terus menerus_. Artinya, setelah Anda menjalankan perintah ini, _proses akan tetap berjalan dan akan terus mendengarkan antrian untuk pekerjaan baru yang masuk_. Ketika ada _pekerjaan baru_ dalam antrian, _proses akan secara otomatis memprosesnya._ Ini berguna untuk aplikasi yang membutuhkan pemrosesan antrian secara berkelanjutan tanpa perlu memulai ulang proses setiap kali.

## Studi Kasus
Saya ingin mengirim email untuk users yg memiliki permission `hr_connect_notified_in`. Mungkin simplenya akan seperti ini.

```php
$email_hr_karyawan = User::whereHas('group.permissions', function ($query) {|
	$query->where('codename', 'hr_connect_notified_in');|
})->pluck('email');

foreach($email_hr_karyawan as $email){
	KaryawanMasukToHR::dispatch($email, $data);
}
```

Kode diatas berjalan dengan baik dan aman.

Tapi.... ternyata yg saya baru pelajari itu bahwa kode diatas itu tanda nya kita looping dan mengirimkan 1 email 1 jobs. bagaimana jika 1000 orang yg menerima?

Baiknya seperti ini...

```php
$email_hr_karyawan = User::whereHas('group.permissions', function ($query) {
	$query->where('codename', 'hr_connect_notified_in');
})->select('email')
->whereNotNull('email')
->groupBy('email')
->get();

$to = $email_hr_karyawan->pluck('email')->toArray();
KaryawanMasukToHR::dispatch($to, $data);
```

di `KaryawanMasukToHR`

```php
public $to;
public $data;

public function __construct($to, $data)
{
	$this->to = $to;
	$this->data = $data;
}

public function handle()
{
	Mail::to($this->to)->send(new AttachExcelToHRMail($this->data));
}
```

Ternyata Mail::to ini dia bisa menerima array. Jadi dengan ini kita bisa ngirim 1 mail ke beberapa orang bersangkutan dalam 1jobs. prosesnya jadi lebih cepat.

Note : gunakan queue:listen, karena proses akan tetap berjalan dan akan terus mendengarkan antrian untuk pekerjaan baru yang masuk.

Date : 29-05-24