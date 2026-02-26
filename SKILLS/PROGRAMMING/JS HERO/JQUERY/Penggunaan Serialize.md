#Tech 
## Introduction
Gue pernah punya case dimana 1 halaman itu bisa 20 an lebih input data. Maka otomatiskan kita harus menangkap data nya seperti ini `let nama = $("#nama").val()`. Nah gue nemu cara yg lebih cepat lagi. Yaitu pakai serialize.

## Apa itu Serialize?
adalah fungsi yg digunakan untuk **mengumpulkan semua nilai dari form.** Jadi kita gaperlu define lagi tuh per inputan nya.

## Implementasi nya
```html
<form id="myForm">
    <label>Nama: <input type="text" name="nama"></label><br><br>
    <label>Email: <input type="email" name="email"></label><br><br>
    <label>Jenis Kelamin:
      <select name="jk">
        <option value="laki">Laki-laki</option>
        <option value="perempuan">Perempuan</option>
      </select>
    </label><br><br>
    <label>Pesan: <textarea name="pesan"></textarea></label><br><br>
    <button type="button" id="submitBtn">Kirim</button>
  </form>

  <div id="output"></div>

  <script>
    $(document).ready(function() {
      $('#submitBtn').click(function() {
        let formData = $('#myForm').serialize();
        
        $.ajax({
          url: '/contoh',
          type: 'POST',
          data: formData,
          success: function(response) {
            $('#output').html(response);
          }
        });
        */
      });
    });
  </script>
```

Bandingkan bila tanpa serialize.

```html
<form id="myForm">
    <label>Nama: <input type="text" name="nama" id="nama"></label><br><br>
    <label>Email: <input type="email" name="email" id="email"></label><br><br>
    <label>Jenis Kelamin:
      <select name="jk" id="jk">
        <option value="laki">Laki-laki</option>
        <option value="perempuan">Perempuan</option>
      </select>
    </label><br><br>
    <label>Pesan: <textarea name="pesan" id="pesan"></textarea></label><br><br>
    <button type="button" id="submitBtn">Kirim</button>
  </form>

  <div id="output"></div>

  <script>
    $(document).ready(function() {
      $('#submitBtn').click(function() {
        let nama = $('#nama').val();
        let email = $('#email').val();
        let jk = $('#jk').val();
        let pesan = $('#pesan').val();
	    let data = {
		    'nama': nama,
		    'email': email,
		    'jk': jk,
		    'pesan': pesan,
	    };
	    
        $.ajax({
          url: '/contoh',
          type: 'POST',
          data: data,
          success: function(response) {
            $('#output').html(response);
          }
        });
        */
      });
    });
  </script>
```

Lebih cepet pakai serialize bukan?
Belum lagi kalau lupa define inputan nya bakal ada error di backend nya!

Date: 28-05-2025