#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Starter|Starter]]
- [[#Read Data|Read Data]]
- [[#Store Data|Store Data]]
- [[#Sinkron Data Ketika Online|Sinkron Data Ketika Online]]
- [[#Update|Update]]
- [[#Remove Data|Remove Data]]
- [[#Kesimpulan|Kesimpulan]]

## Introduction
Kenapa saya ingin menulis tentang _Indexed DB?_

Saya mendapatkan project dari PAS dari permintaan manager(bu Linda) bahwa saya dipercaya untuk membuat project untuk departemen ENG yg bernama SDP (Sub Distribution Panel). SDP ini kan titik device nya bisa dimana aja ya. Nah problemnya itu di koneksi internet yg tidak stabil. Indexed DB lah yang bisa mengatasi problem tersebut.

IndexedDB adalah sebuah database di dalam browser yang memungkinkan aplikasi web untuk menyimpan data secara terstruktur dan melakukan pencarian yang efisien. Berbeda dengan penyimpanan lokal sederhana, seperti localStorage, IndexedDB dapat menyimpan sejumlah besar data dan mendukung operasi transaksi, indeks, dan query yang kompleks.

Berikut adalah beberapa fitur utama dari IndexedDB:
1. **Penyimpanan Data Besar**: Dapat menyimpan data dalam jumlah yang jauh lebih besar dibandingkan localStorage.
2. **Struktur Data**: Memungkinkan penyimpanan objek JavaScript, bukan hanya string.
3. **Transaksi**: Mendukung transaksi untuk memastikan integritas data.
4. **Indeks**: Memungkinkan pencarian data yang lebih cepat dengan membuat indeks.
5. **Asynchronous API**: Operasinya bersifat non-blocking, sehingga tidak akan mengganggu kinerja antarmuka pengguna.

IndexedDB sangat berguna untuk aplikasi web yang memerlukan penyimpanan data yang kompleks dan besar, seperti **aplikasi offline**, aplikasi pemrosesan data, atau aplikasi dengan data pengguna yang banyak.

## Starter
```javascript
const dbName = 'MesinDB';
const storeName = 'mesinStore';
let db;

function initDB() {
    const request = indexedDB.open(dbName, 1);

    request.onupgradeneeded = function (event) {
        db = event.target.result;
        db.createObjectStore(storeName, { keyPath: 'id' });
    };

    request.onsuccess = function (event) {
        db = event.target.result;
        console.log('Database berhasil dibuka');
        loadDataFromIndexedDB();
    };

    request.onerror = function (event) {
        console.error('Kesalahan database:', event.target.errorCode);
    };
}
```

Penjelasan
- `indexedDB.open(dbName, 1)`: intinya disini kita harus open database yg kita buat (ini nama databasenya bebas ya) dan versi `1`.
- `onupgradeneeded` dipanggil ketika database perlu diupgrade, seperti saat pertama kali dibuat atau jika versi database lebih rendah dari yang diminta.
- `db = event.target.result;`: Menyimpan referensi ke database yang baru dibuat atau diupgrade.
- `db.createObjectStore(storeName, { keyPath: 'id' });`: Membuat object store baru dengan nama `mesinStore` dan menentukan bahwa `id` adalah `keyPath`, yang berarti setiap objek dalam store harus memiliki properti `id` yang unik.
- `onsuccess` dan `onerror` itu bila di ajax mirip `success` dan `error`
- Setelah di bungkus pakai function jangan lupa di panggil!

## Read Data
```javascript
function loadDataFromIndexedDB() {
    const transaction = db.transaction([storeName]);
    const store = transaction.objectStore(storeName);
    const request = store.getAll();

    request.onsuccess = function (event) {
        console.log('Semua data:', event.target.result);
    };
}
```

Penjelasan:
- pertama tama kita harus buat nama transaction nya dulu
- mengakses objectstore
- Menggunakan metode `getAll` untuk mengambil semua data yang ada di object store. (bentuknya array)
- request on success itu dijalankan ketika operasi dinyatakan berhasil.

## Store Data
```javascript
function addDataToIndexedDB(data) {
    const transaction = db.transaction([storeName], 'readwrite');
    const store = transaction.objectStore(storeName);
    store.add(data);
}
```

Penjelasan:
- Seperti sebelumnya buat transaksi dan objectStore
- disitu kita menetapkan `readwrite` yang artinya bisa read dan create

## Sinkron Data Ketika Online
```javascript
function syncData() {
    const transaction = db.transaction([storeName]);
    const store = transaction.objectStore(storeName);
    const request = store.getAll();

    request.onsuccess = function(event) {
        const dataToSync = event.target.result;

        if (dataToSync.length > 0) {
            dataToSync.forEach(item => {
                $.ajax({
                    url: '/sdp/master/mesin',
                    type: 'POST',
                    data: item,
                    success: function(response) {
                        table();
                        removeFromLocalDB(item.id);
                        console.log("Sukses menyinkronkan data!");
                    },
                    error: function(xhr) {
                        console.error("Gagal menyinkronkan data:", xhr);
                    }
                });
            });
        }
    };
}
```

Penjelasan:
- Sama seperti sebelumnya kita harus buat transaction dan objectStore nya
- di bagian request ibaratnya ini kita lagi nge-query
- nah setelah data ditemukan (onsuccess) maka data disinkronkan pakai ajax.
- Intinya ketika offline disimpan di browser, setelah online baru kirim ke server dengan syarat tidak boleh di refresh.

## Update
```javascript
function updateDataInIndexedDB(data) {
    const transaction = db.transaction([storeName], 'readwrite');
    const store = transaction.objectStore(storeName);
    const request = store.put(data);

    request.onsuccess = function() {
        console.log('Data berhasil diperbarui di IndexedDB');
    };

    request.onerror = function(event) {
        console.error('Gagal memperbarui data di IndexedDB:', event.target.error);
    };
}
```

Penjelasan:
- gunakan put untuk perbarui data

## Remove Data
```javascript
function removeFromLocalDB(id) {
    const transaction = db.transaction([storeName], 'readwrite');
    const store = transaction.objectStore(storeName);
    store.delete(id);
}
```

Penjelasan:
- readwrite bisa digunakan untuk crud
- gunakan delete untuk menghapus!

## Kesimpulan
- IndexedDB itu cocok buat user yg tidak stabil koneksinya atau aplikasi offline.
- IndexedDB itu database nya browser.
- Flownya
    - open database dg version yg sama
    - buat transaksi dengan tipe nya seperti readonly atau readwrite
    - buat objectStore
    - object nya diquery

Date: 24-10-2024