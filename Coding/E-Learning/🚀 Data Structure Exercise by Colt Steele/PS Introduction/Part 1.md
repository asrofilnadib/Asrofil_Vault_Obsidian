## 1️⃣ sameFrequency

**Deskripsi:**  
Buatlah fungsi `sameFrequency(num1, num2)` yang menerima dua angka.  
Fungsi harus mengembalikan `true` jika kedua angka tersebut memiliki **frekuensi digit yang sama** (urutan boleh beda). Jika tidak, kembalikan `false`.

**Contoh:**

```javascript
sameFrequency(182, 281)         // true   (karena 1,8,2 sama-sama muncul sekali)
sameFrequency(34, 14)           // false  (digit 3 dan 4 beda dengan 1 dan 4)
sameFrequency(3589578, 5879385) // true
sameFrequency(22, 222)          // false  (panjang beda, jumlah digit 2 tidak sama)

```

---

## 2️⃣ areThereDuplicates (Frequency Counter)

**Deskripsi:**  
Buatlah fungsi `areThereDuplicates(...args)` yang menerima jumlah argumen tak terbatas.  
Fungsi harus mengembalikan `true` jika ada nilai yang **muncul lebih dari sekali**, dan `false` jika semua nilai unik.

**Contoh:**

```javascript
areThereDuplicates(1, 2, 3)        // false
areThereDuplicates(1, 2, 2)        // true
areThereDuplicates('a', 'b', 'c')  // false
areThereDuplicates('a', 'b', 'a')  // true
```

---

## 3️⃣ areThereDuplicates (Multiple Pointers)

**Deskripsi:**  
Tulis fungsi `areThereDuplicates(...args)` yang:

- menerima argumen tak terbatas,
    
- **mengurutkan argumen** dulu,
    
- kemudian memeriksa apakah ada dua nilai yang sama berdampingan.
    

Jika ada, return `true`, kalau semua beda, return `false`.

**Contoh:**

```javascript
areThereDuplicates(1, 2, 3)        // false
areThereDuplicates(1, 2, 2)        // true
areThereDuplicates('a', 'b', 'c')  // false
areThereDuplicates('a', 'c', 'a')  // true
```

---

## 4️⃣ areThereDuplicates (One Liner - Set)

**Deskripsi:**  
Buatlah fungsi `areThereDuplicates(...args)` yang menggunakan **Set**.  
Kembalikan `true` jika jumlah argumen **berbeda** dengan ukuran `Set` (artinya ada duplikat).

**Contoh:**
```javascript
areThereDuplicates(1, 2, 3)        // false
areThereDuplicates(1, 2, 2)        // true
areThereDuplicates('a', 'b', 'c')  // false
areThereDuplicates('a', 'b', 'a')  // true
```