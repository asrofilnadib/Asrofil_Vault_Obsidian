#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
Berawal dari codingan kalista yg dibantu mas dewa. Saya penasaran untuk membuat preview image sebelum Upload. Berikut codingannya :

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title>Latihan JS</title>
	<link rel="stylesheet" href="<https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css>">
</head>
<body>
	<input type="file" accept="image/*" id="file" onchange="loadFile(event)" class="form-control">
	<div class="mt-5 text-center">
		<img class="img-fluid rounded" id="output" width="510" />
	</div>
	<script type="text/javascript">
	let loadFile = function(event) {
		let image = document.getElementById('output');
		image.src = URL.createObjectURL(event.target.files[0]);
	};
	</script>
</body>
</html>
```

Referensi : [https://www.webtrickshome.com/forum/how-to-display-uploaded-image-in-html-using-javascript](https://www.webtrickshome.com/forum/how-to-display-uploaded-image-in-html-using-javascript)