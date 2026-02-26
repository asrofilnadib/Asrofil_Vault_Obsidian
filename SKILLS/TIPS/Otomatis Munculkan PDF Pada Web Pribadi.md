![[Pasted image 20250524224423.png]]
#Tech 
## Introduction
Gue tertarik untuk membuat website pribadi yg ngga bertele-tele. Menampilkan halaman yg interaktif, menampilkan skills yg memadai. Oke that's good, but too much time buat HRD ngeliat website lo -IMO.

## Referensi Landing Page
Awal mula kenapa gue rombak dari web pribadi gue yg interaktif, menjadi simple, menjadi showing pdf aja. Contoh referensi nya...

![[Bene_Company_Profile_2025.pdf]]

## Implementasi
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Andaru Triadi - Portfolio</title>
<meta name="description" content="Software Enginner yang memiliki pengalaman lebih dari 3 tahun dalam membuat dan mengembangkan aplikasi berbasis web.">
<style>
	body, html {
		margin: 0;
		padding: 0;
		height: 100%;
	}
	iframe {
		width: 100%;
		height: 100%;
		border: none;
	}
</style>
</head>
<body>
    <iframe 
		src="https://docs.google.com/gview?url=andarutriadi.com/assets/portofolio_andaru_software_engineer.pdf&embedded=true" 
		style="width:100%; height:100vh;" 
		frameborder="0">
	</iframe>
</body>
</html>
```

Note: Gunakan `embedded=true` agar tidak error "google refuse to connect! dan pastikan url pdf nya bisa diakses";

Date: 24/5/25