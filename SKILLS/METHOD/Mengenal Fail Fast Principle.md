#Tech 
## Introduction
**Fail Fast Principle** adalah konsep dalam pemrograman yang menyarankan agar **sistem segera menunjukkan kesalahan (error)** secepat mungkin saat terjadi kondisi yang tidak valid atau kesalahan input, **sebelum menimbulkan dampak lebih besar**.

Tujuan utama:
- Memudahkan **debugging**
- Mencegah **masalah yang lebih besar**
- Meningkatkan **keamanan dan stabilitas sistem**

## Implementasi
```php
public function store(Request $request)
{
	$request->validate([
		'name' => 'required|string|max:255',
		'email' => 'required|email|unique:users',
		'password' => 'required|min:8|confirmed',
	]);

	$user = User::create([
		'name' => $request->name,
		'email' => $request->email,
		'password' => Hash::make($request->password),
	]);

	return response()->json([
		'message' => 'User berhasil dibuat',
		'data' => $user
	], 201);
}
```

disisi frontend:
```html
<form id="userForm">
    <input type="text" id="name" placeholder="Nama" required>
    <input type="email" id="email" placeholder="Email" required>
    <input type="password" id="password" placeholder="Password" required>
    <input type="password" id="password_confirmation" placeholder="Konfirmasi Password" required>
    <button type="submit">Daftar</button>
</form>

<div id="message"></div>
```

```javascript
$(document).ready(function() {
    $('#userForm').on('submit', function(e) {
        e.preventDefault();

        const formData = {
            name: $('#name').val().trim(),
            email: $('#email').val().trim(),
            password: $('#password').val(),
            password_confirmation: $('#password_confirmation').val()
        };

        // ✅ FAIL FAST: Validasi sederhana di frontend
        if (!formData.name || !formData.email || !formData.password) {
            $('#message').text('Semua field wajib diisi');
            return;
        }

        if (formData.password !== formData.password_confirmation) {
            $('#message').text('Password tidak cocok');
            return;
        }

        // ✅ Kirim data ke backend
        $.ajax({
            url: '/api/users',
            method: 'POST',
             formData,
            success: function(response) {
                $('#message').text(response.message).css('color', 'green');
                $('#userForm')[0].reset(); // Reset form
            },
            error: function(xhr) {
                // ✅ Tampilkan error dari backend
                let errorMessage = 'Terjadi kesalahan';
                if (xhr.responseJSON && xhr.responseJSON.message) {
                    errorMessage = xhr.responseJSON.message;
                } else if (xhr.responseJSON && xhr.responseJSON.errors) {
                    // Ambil error pertama
                    const errors = xhr.responseJSON.errors;
                    errorMessage = Object.values(errors)[0][0]; // Ambil pesan pertama
                }
                $('#message').text(errorMessage).css('color', 'red');
            }
        });
    });
});
```

Date: 06-10-2025