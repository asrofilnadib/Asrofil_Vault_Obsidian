#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Siapkan Input Form & Image Container|Siapkan Input Form & Image Container]]
- [[#Logic nya|Logic nya]]
- [[#Hasilnya|Hasilnya]]

## Introduction
Gue dapet case ketika show gambar di project "Web Generator" punya temen gue. Gue nemu cara yg sangat sederhana

## Siapkan Input Form & Image Container
```html
<div class="col-4">
	<div class="form-group">
		<label>Upload Logo</label>
		<input type="file" class="form-control" id="foto_logo">
	</div>    
</div>

<div id="logoPreviewSidebar"></div>
```
Penjelasan: 
1. Siapkan input form gambar
2. dan container foto nya (tempat show temporary image)

## Logic nya
```javascript
$(document).on("change", "#foto_logo", function(event){
    const file = event.target.files[0];
    const reader = new FileReader();

    reader.onload = function(e) {
	    $('#logoPreviewSidebar').html(`<img src="${e.target.result}">`);
    }

    reader.readAsDataURL(file);
});
```

Penjelasan:
1. `files[0]` artinya hanya mengambil file pertama (kalau input-nya support multiple files).
2. `FileReader` untuk membaca isi file dalam bentuk base64 (Data URL).
3. `reader.readAsDataURL(file);` membaca file dan hasilnya akan dikembalikan dalam format Data URL (base64), cocok untuk preview gambar.

## Hasilnya
![[Pasted image 20250426154154.png]]

Date: 26-04-2025