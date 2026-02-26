#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
Saya ingin melihat password yg saya masukkan supaya lebih yakin dengan apa yg saya ingin ubah datanya. Sebelumnya sudah menggunakan js vanilla, kali ini jquery. malah lebih ringkas ygy.... Berikut kodingannya :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Latihan</title>
    <link href="<https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css>" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="row">
            <div class="co-md-4 mx-auto mt-5">
                <div class="input-group mb-3">
                    <input type="password" class="form-control" placeholder="Input password" id="passwordForm">
                    <button class="btn btn-outline-secondary" type="button" id="seePassword">Lihat password</button>
                </div>
            </div>
        </div>
    </div>

<script src="<https://code.jquery.com/jquery-3.7.1.min.js>" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
<script>
$(document).on("click", "#seePassword", function(){
    let type = $("#passwordForm").attr("type");
    if(type === "password") {
        $("#passwordForm").attr("type", "text");
        $(this).text("Sembunyikan password");
    } else {
        $("#passwordForm").attr("type", "password");
        $(this).text("Lihat password");
    }
});
</script>
</body>
</html>
```

Date : 18-06-2024