#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
Saya ingin melihat password yg saya masukkan supaya lebih yakin dengan apa yg saya ingin ubah datanya. Berikut kodingannya :

```html
<div class="form-group">
	<label>Ulangi Password</label> <br>
	<input id="name" type="password" name="confirmation_password" class="form-control password_input" name="confirmation_password">
	<a href="javascript:;" class="preview_icon">Show Password</a>
</div>

<script>
    const PASSWORD = 'password'
    const TEXT = 'text'

    const passwordIcon = document.querySelector('.preview_icon')
    const passwordField = document.querySelector('.password_input')

    function togglePassword () {
        if (passwordField.type === PASSWORD) {
            passwordField.type = TEXT
        } else {
            passwordField.type = PASSWORD
        }
    }

    passwordIcon.addEventListener('click', togglePassword);
</script>
```

Referensi : [https://medium.com/@GistCoding/simple-show-hide-password-with-vanilla-javascript-d530e803b156](https://medium.com/@GistCoding/simple-show-hide-password-with-vanilla-javascript-d530e803b156)