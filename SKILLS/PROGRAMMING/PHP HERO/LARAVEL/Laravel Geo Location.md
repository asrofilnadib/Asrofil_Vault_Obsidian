#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Instalasi|Instalasi]]
- [[#Contoh Penggunaan|Contoh Penggunaan]]

## Introduction
Saya menggunakan package ini ketika saya mendapatkan joki dari Dimas (teman kampus) untuk membuat aplikasi absen dengan tambahan fitur track kota, ip, dan longitude, latitude.

## Instalasi
```
composer require adrianorosa/laravel-geolocation
```

## Contoh Penggunaan
```php
$geo = GeoLocation::lookup();
	Absensi::create([
	'nama' => $request->nama,
	'kelas' => $request->kelas,
	'foto' => str_replace(' ', '_', $request->nama).$request->kelas.$request->jurusan.$request->mata_pelajaran.date('m.d.y').".jpg",
	'jurusan' => str_replace(' ', '-', $request->jurusan),
	'guru' => $username->name,
	'nrp' => $username->username,
	'mata_pelajaran' => $request->mata_pelajaran,
	'waktu' => Carbon::now()->locale('id')->isoFormat('LLLL'),
	'ip_device' => $geo->getIp(),
	'kota' => $geo->getCity(),
	'latitude' => $geo->getLatitude(),
	'longitude' => $geo->getLongitude()
]);
```