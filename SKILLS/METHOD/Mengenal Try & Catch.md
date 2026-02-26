#Tech
# Table of Content
- [[#Introduction|Introduction]]
- [[#Mekanisme|Mekanisme]]
- [[#Kesimpulan|Kesimpulan]]

## Introduction
Try & Catch ini struktur pemrograman dimana biasanya digunakan apabila kode rentan terhadap error.

## Mekanisme
Mekanisme `try-catch` bekerja dengan cara yang cukup sederhana:
- **Blok Try:** Pertama-tama, kode di dalam blok `try` akan dijalankan. Jika semua berjalan normal, blok `catch` akan diabaikan, dan eksekusi program akan dilanjutkan seperti biasa.
- **Blok Catch:** Namun, jika terjadi eksepsi selama eksekusi blok `try`, proses normal akan terinterrupt, dan kontrol akan langsung beralih ke blok `catch`. Di sinilah programmu dapat merespons kesalahan dengan cara yang telah didefinisikan.

## Implementasi

```php
try {
	$user = Auth::user();
	$file = $request->file('picture');
	$filename = $file->hashName();
	$path = $file->storeAs('images/profile', $filename, 'public');
	$oldPicture = $user->picture;
	$user->picture = $filename;
	$user->save();
	
	if ($oldPicture !== 'default-placeholder.png') {
		Storage::disk('public')->delete('images/profile/' . $oldPicture);
	}

	return response()->json([
		'success' => true,
		'message' => 'Foto profil berhasil diperbarui!',
		'new_picture_url' => Storage::url($path)
	]);

} catch (\Exception $e) {
	\Log::error('Profile picture upload failed: ' . $e->getMessage());
	return response()->json([
		'success' => false,
		'message' => 'Terjadi kesalahan internal saat mengunggah.'
	], 500); 
}
```

## Kesimpulan
Try-catch adalah struktur penting dalam pemrograman yang membantu mengelola kesalahan dan menjamin stabilitas aplikasi. Dengan penanganan kesalahan yang tepat, kita dapat menghindari berbagai masalah.

Date: 29-04-2025