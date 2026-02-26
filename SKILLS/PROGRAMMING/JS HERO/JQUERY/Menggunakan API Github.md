#Tech 
# Table of Content
- [[#GET API Github|GET API Github]]
- [[#Looping|Looping]]

## GET API Github
Pertama kita perlu load ajax terlebih dahulu. Boleh di header atau footer. Usahakan di header ya.

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.0/jquery.min.js>"></script>
```

Selanjutnya kita akan buat tampilan seperti ini :

<img src="Github API.png" width="350" align="left">











Berikut contoh codingannya :

```html
<div class="container mt-5 mb-5">
      <div class="row">
        <div class="col-lg-6 mx-auto">
          <div class="card shadow">
            <div class="card-body p-4">
              <center>
                <h1>Github API</h1>
                <img id="imgU" class="img-fluid rounded-circle">
                <h1 id="name" class="mt-3"></h1>
                <p id="login" class="mt-3"></p>
                <p id="repository" class="mt-3"></p>
              </center>
              <p id="bio"></p>
              <a id="websiteU"></a>
            </div>
            <div class="card-footer">
              <p id="location"></p>
            </div>
          </div>
        </div>
      </div>
</div>

```

Terakhir kita coding jquery Ajax Nya

```html
<script>
	let url = "<https://api.github.com/users/andarutr>";
	$.get(url, function(data, status){
		$("#imgU").attr("src", data.avatar_url);
		$("#name").text(data.name);
		$("#repository").text("Total Repository : " + data.public_repos);
		$("#login").text("Username : " + data.login);
		$("#bio").text(data.bio);
		$("#websiteU").text(data.blog).attr("href", data.blog);
		$("#location").text(data.location);
	});
</script>
```

## Looping
Kode Lengkap :

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Latihan Ajax</title>
    <link href="<https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css>" rel="stylesheet">
    <script src="<https://ajax.googleapis.com/ajax/libs/jquery/3.4.0/jquery.min.js>"></script>
  </head>
  <body>
    <div class="container mt-5 mb-5">
      <div class="row">
        <div class="col-lg-6 mx-auto">
          <div class="card shadow">
            <div class="card-body p-4">
              <p id="name"></p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <script>
    $(document).ready(function() {
        $.ajax({
            url: '<https://api.github.com/users/andarutr/repos>',
            method: 'GET',
            dataType: 'json',
            success: function(data) {
                $.each(data, function(index, repo) {
                    $('#name').append('<p>' + repo.name + '</p><br><p>'+ repo.description + '</p>');
                });
            },
            error: function(xhr, status, error) {
                console.error('Error:', error);
            }
        });
    });
    </script>
  </body>
</html>
```

Date : 17-02-2024