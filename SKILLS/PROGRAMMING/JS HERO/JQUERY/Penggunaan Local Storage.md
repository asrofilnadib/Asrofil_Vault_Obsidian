![[Penggunaan LocalStorage.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Local Storage|Local Storage]]
- [[#Logic HTML|Logic HTML]]
- [[#Logic JS|Logic JS]]

## Introduction
Studi kasus ini saya disuruh membuat fitur cart. Awalnya saya buat ada interaksi ajax ke backend. Namun benar-benar lemot. Saya sadar bahwa ngga semua hal kita pakai backend. Seperti studi kasus ini, ternyata lebih cepat bila menggunakan localStorage.

## Local Storage
`localStorage` adalah bagian dari API Web Storage yang disediakan oleh browser web modern. Ini memungkinkan pengembang web untuk menyimpan data secara lokal di dalam browser pengguna. Data yang disimpan menggunakan `localStorage` akan tetap ada bahkan setelah pengguna menutup browser atau mengakhiri sesi.

Berikut adalah beberapa fitur utama dari `localStorage`:
1. **Kapasitas**: `localStorage` biasanya memiliki kapasitas penyimpanan yang lebih besar daripada cookies, umumnya sekitar 5-10 MB per domain.
2. **Antarmuka JavaScript**: `localStorage` dapat diakses dan dimanipulasi menggunakan JavaScript melalui antarmuka yang sederhana, seperti menyimpan, mengambil, dan menghapus item.
3. **Penggunaan**: Data yang disimpan dalam `localStorage` disimpan sebagai pasangan kunci-nilai (key-value pairs). Anda dapat menyimpan data primitif seperti string, angka, atau boolean, serta objek JavaScript yang diubah menjadi string menggunakan metode `JSON.stringify()`.
4. **Scope**: Data dalam `localStorage` spesifik untuk setiap domain, yang berarti data dari satu domain tidak dapat diakses oleh domain lain.
5. **Penghapusan**: Data dalam `localStorage` dapat dihapus oleh pengguna melalui pengaturan browser atau oleh skrip JavaScript yang dijalankan di dalam halaman.

Contoh penggunaan `localStorage` adalah menyimpan preferensi pengguna, status login, atau data aplikasi kecil yang tidak memerlukan penyimpanan server. Namun, perlu diingat bahwa `localStorage` tidak cocok untuk menyimpan data rahasia atau sensitif karena data tersebut dapat diakses oleh pengguna melalui pengembang alat browser.

## Logic HTML
```html
<button class="btn btn-icon" onClick="addToCart('${row.id}', '${row.nik}', '${row.nama}', '${row.kode_bagian}')">
    <i class="mdi mdi-cart cartId" data-cart="${row.id}"></i>
</button>
```

Penjelasan :
- Terdapat sebuah tombol dengan kelas CSS `btn btn-icon`.
- Ketika tombol tersebut diklik (`onClick`), fungsi `addToCart()` dipanggil dengan beberapa parameter (`row.id`, `row.nik`, `row.nama`, `row.kode_bagian`) (menggunakan datatable).
- Di dalam tombol, terdapat ikon keranjang (`<i>` dengan kelas `mdi mdi-cart`) yang mungkin menunjukkan ada keranjang kosong atau tidak.

## Logic JS
```jsx
// Deklarasi variabel untuk menyimpan data keranjang belanja
var cartContainer = JSON.parse(localStorage.getItem("karyawan_aktif_cartContainer")) || [];

// Fungsi untuk menambahkan item ke keranjang belanja
function addToCart(id, nik, nama, dept) {
    // Mendapatkan data keranjang belanja dari localStorage atau menginisialisasi dengan array kosong jika belum ada
    var cartContainer = JSON.parse(localStorage.getItem("karyawan_aktif_cartContainer")) || [];

    // Mencari indeks dari item dengan id yang sama di dalam keranjang
    var foundIndex = cartContainer.findIndex(function(cart) {
        return cart.id === id;
    });

    // Jika item dengan id yang sama ditemukan, maka perbarui detailnya, jika tidak tambahkan item baru
    if (foundIndex !== -1) {
        cartContainer[foundIndex] = {
            id: id,
            nik: nik,
            nama: nama,
            dept: dept,
            alasan_keluar: '',
            tanggal_keluar: ''
        };
    } else {
        cartContainer.push({
            id: id,
            nik: nik,
            nama: nama,
            dept: dept,
            alasan_keluar: '',
            tanggal_keluar: ''
        });
    }

    // Simpan kembali keranjang belanja ke localStorage setelah diperbarui
    localStorage.setItem("karyawan_aktif_cartContainer", JSON.stringify(cartContainer));

    // Perbarui tampilan jumlah item di keranjang
    $('#cart-count').text(cartContainer.length);

    // Kemungkinan fungsi untuk memperbarui tampilan tabel keranjang belanja
    updateCartTable();
}

function updateCartTable(data) {
    // Mengosongkan isi tbody dari tabel keranjang belanja
    $('#cart-table tbody').empty();

    // Mendapatkan data keranjang belanja dari localStorage atau menggunakan data yang diberikan jika ada
    var cartContainer = JSON.parse(localStorage.getItem("karyawan_aktif_cartContainer")) || [];
    var cartData = data || cartContainer;

    // Iterasi melalui setiap item di keranjang belanja dan menambahkannya ke dalam tabel
    cartData.forEach(function(cart) {
        // Menambahkan baris baru ke dalam tbody tabel
        $('#cart-table tbody').append(`
            <tr>
                <td>${cart.nama}</td> <!-- Menampilkan nama -->
                <td>${cart.nik}</td> <!-- Menampilkan NIK -->
                <td>${cart.dept}</td> <!-- Menampilkan departemen -->
                <td>
                    <select class="form-select alasanKeluar" data-cart-id="${cart.id}" onChange="updateReason('${cart.id}', this.value)">
                        <option value="">Pilih</option>
                        <option value="Resign" ${cart.alasan_keluar === 'Resign' ? 'selected' : ''}>Resign</option>
                        <option value="Habis Kontrak" ${cart.alasan_keluar === 'Habis Kontrak' ? 'selected' : ''}>Habis Kontrak</option>
                        <option value="Kabur" ${cart.alasan_keluar === 'Kabur' ? 'selected' : ''}>Kabur</option>
                    </select>
                </td>
                <td>
                    <input type="date" class="form-control tglKeluar" data-cart-id="${cart.id}" onChange="updateTglKeluar('${cart.id}', this.value)" value="${cart.tanggal_keluar || ''}">
                </td>
                <td>
                    <button class="btn btn-sm btn-danger removeCartId" onClick="removeFromCart('${cart.id}')">
                        <i class="mdi mdi-delete"></i>
                    </button>
                </td>
            </tr>
        `);
    });

    // Reload tabel tanpa memperbarui pagination
    table2.api().draw(false);
}
```

Penjelasan :
- Variabel `cartContainer` dideklarasikan untuk menyimpan data keranjang belanja. Fungsinya adalah mengurai data dari `localStorage` jika sudah ada, atau menginisialisasi dengan array kosong jika belum ada.
- Fungsi `addToCart()` ditujukan untuk menambahkan item baru ke keranjang belanja:
    - Menerima beberapa parameter: `id`, `nik`, `nama`, dan `dept`.
    - Mengecek apakah item dengan `id` yang sama sudah ada di dalam keranjang. Jika ya, maka detailnya diperbarui, jika tidak, item baru ditambahkan.
    - Setelah perubahan, keranjang belanja disimpan kembali ke `localStorage`.
    - Jumlah item di keranjang yang ditampilkan diperbarui.
- Data keranjang belanja diambil dari `localStorage` menggunakan `JSON.parse(localStorage.getItem("karyawan_aktif_cartContainer"))`. Jika tidak ada data yang tersimpan di `localStorage`, maka akan digunakan array kosong.
- Jika ada data yang diberikan sebagai parameter `data`, maka data tersebut akan digunakan. Jika tidak, data dari `localStorage` akan digunakan.
- Selanjutnya, fungsi melakukan iterasi melalui setiap item dalam data keranjang belanja dan menambahkannya ke dalam tabel menggunakan fungsi `append()` dari jQuery.
- Setiap item dari keranjang belanja ditampilkan dalam sebuah baris tabel dengan informasi seperti nama, NIK, departemen, alasan keluar, tanggal keluar, dan tombol untuk menghapus item dari keranjang.
- Terakhir, setelah semua item ditambahkan ke tabel, fungsi melakukan reload tabel tanpa memperbarui pagination menggunakan `table2.api().draw(false)`. Hal ini membantu agar data baru ditampilkan tanpa mempengaruhi navigasi halaman pada tabel.

Date : 04-06-2024