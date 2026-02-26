## 🔹 Soal 1 – Average Pair

**Problem Statement:**  
Buatlah sebuah fungsi bernama `averagePair` yang menerima **dua parameter**:
1. `arr` → sebuah array integer yang sudah **diurutkan secara ascending** (sorted).
2. `num` → sebuah target number.

Fungsi harus mengembalikan `true` jika ada **pasangan angka** dalam array tersebut yang memiliki rata-rata (`average`) sama dengan `num`. Jika tidak ada, maka kembalikan `false`.

**Constraints & Rules:**
- Gunakan pendekatan **two-pointer** (satu pointer di awal array, satu di akhir).
- Jangan gunakan nested loop brute force.
- Time complexity diharapkan **O(n)**.
- Space complexity **O(1)**.

```javascript
averagePair([1,2,3],2.5) // true  -> (2+3)/2 = 2.5 
averagePair([1,3,3,5,6,7,10,12,19],8) // true -> (6+10)/2 = 8 
averagePair([-1,0,3,4,5,6], 4.1) // false averagePair([],4) // false
```

---

## 🔹 Soal 2 – Is Subsequence (Iterative)

**Problem Statement:**  
Buatlah sebuah fungsi bernama `isSubsequence` yang menerima **dua string**:

1. `str1` → string yang lebih pendek.
    
2. `str2` → string yang lebih panjang.
    

Fungsi harus mengembalikan `true` jika semua karakter di `str1` muncul di `str2` **dengan urutan yang sama** (tidak harus bersebelahan). Jika tidak, kembalikan `false`.

**Constraints & Rules:**

- Implementasikan solusi menggunakan **iterasi** (loop while/for).
    
- Gunakan dua pointer untuk melacak posisi pada kedua string.
    
- Time complexity diharapkan **O(n + m)**, di mana `n` adalah panjang `str1` dan `m` panjang `str2`.
    
- Space complexity **O(1)**.
    

**Example:**

```javascript
isSubsequence('hello', 'hello world') // true 
isSubsequence('sing', 'sting')        // true 
isSubsequence('abc', 'abracadabra')   // true 
isSubsequence('abc', 'acb')           // false
```
---

## 🔹 Soal 3 – Is Subsequence (Recursive)

**Problem Statement:**  
Buatlah versi **recursive** dari fungsi `isSubsequence` yang menerima parameter `str1` dan `str2` dengan aturan yang sama seperti soal sebelumnya.

**Constraints & Rules:**

- Gunakan pendekatan **rekursi** (tidak menggunakan loop).
    
- Pada setiap langkah:
    
    - Jika `str1` kosong → return `true`.
        
    - Jika `str2` kosong → return `false`.
        
    - Jika karakter pertama `str1` sama dengan karakter pertama `str2`, lakukan rekursi dengan memotong kedua string.
        
    - Jika tidak sama, lakukan rekursi dengan hanya memotong `str2`.
        
- Time complexity tetap **O(n + m)**, tetapi **Space complexity > O(1)** karena penggunaan stack rekursi.
    

**Example:**

`isSubsequence('hello', 'hello world') // true isSubsequence('sing', 'sting')        // true isSubsequence('abc', 'abracadabra')   // true isSubsequence('abc', 'acb')           // false`