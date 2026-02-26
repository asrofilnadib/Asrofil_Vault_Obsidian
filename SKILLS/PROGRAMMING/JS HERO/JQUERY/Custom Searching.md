![[Custom Search Datatable.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Coding|Coding]]

## Introduction
Pada case ini saya lagi buat portfolio, menggunakan template metronic. Namun tidak muncul kotak pencarian nya? nah saya mau ngide custom search Kira kira code nya akan seperti ini.

## Coding
```html
<div class="col-md-2 mb-3">
	<label for="form-label">Tanggal</label>
	<input type="date" class="form-control" id="tanggalFilter">
</div>
```

```javascript
$("#tanggalFilter").change(function(){
    let search = $(this).val();
    $("#tableTransaksi").columns(1).search(search).draw();
});
```

Date: 14-07-2024