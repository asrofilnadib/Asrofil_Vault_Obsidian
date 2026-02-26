r## 🔹 Soal 1 – Max Subarray Sum

**Problem Statement:**  
Buatlah fungsi bernama `maxSubarraySum` yang menerima:

1. `arr` → array integer (bisa positif atau negatif).
    
2. `num` → integer yang menunjukkan panjang subarray.
    

Fungsi harus mengembalikan **jumlah maksimum** dari subarray berukuran `num` yang terdapat di dalam `arr`. Jika panjang array lebih kecil dari `num`, kembalikan `null`.

**Constraints & Rules:**

- Gunakan teknik **sliding window** untuk mencapai **O(n)**.
    
- Jangan gunakan nested loop brute force `O(n * k)`.
    

**Example:**

```javascript
maxSubarraySum([100,200,300,400], 2) // 700 
maxSubarraySum([1,4,2,10,23,3,1,0,20], 4) // 39 
maxSubarraySum([-3,4,0,-2,6,-1], 2) // 5 
maxSubarraySum([2,3], 3) // null
```

---

## 🔹 Soal 2 – Min Subarray Length

**Problem Statement:**  
Buatlah fungsi bernama `minSubArrayLen` yang menerima:

1. `nums` → array integer **positif**.
    
2. `sum` → target integer.
    

Fungsi harus mengembalikan panjang minimum dari sebuah **subarray kontigu** yang jumlahnya ≥ `sum`. Jika tidak ada subarray yang memenuhi, kembalikan `0`.

**Constraints & Rules:**

- Angka dalam `nums` semuanya **positif** (tidak ada negatif).
    
- Gunakan teknik **sliding window** dengan dua pointer (`start` dan `end`).
    
- Time complexity diharapkan **O(n)**.
    

**Example:**
```javascript
minSubArrayLen([2,3,1,2,4,3], 7) // 2 -> [4,3] 
minSubArrayLen([2,1,6,5,4], 9)   // 2 -> [5,4] 
minSubArrayLen([3,1,7,11,2,9,8,21,62,33,19], 52) // 1 -> [62] 
minSubArrayLen([1,4,4], 4)       // 1 
minSubArrayLen([1,1,1,1,1,1,1,1], 11) // 0
```

---

## 🔹 Soal 3 – Find Longest Substring

**Problem Statement:**  
Buatlah fungsi bernama `findLongestSubstring` yang menerima sebuah string `str` dan mengembalikan panjang substring terpanjang yang memiliki karakter-karakter unik (tidak ada karakter yang berulang di dalam substring tersebut).

**Constraints & Rules:**

- Gunakan pendekatan **sliding window** dengan hash map/object untuk menyimpan posisi karakter.
    
- Time complexity diharapkan **O(n)**.
    

**Example:**
```javascript
findLongestSubstring('')                 // 0 
findLongestSubstring('rithmschool')      // 7 -> "rithmsc" 
findLongestSubstring('thisisawesome')    // 6 -> "isawes" 
findLongestSubstring('thecatinthehat')   // 7 -> "hecatin" 
findLongestSubstring('bbbbbb')           // 1 
findLongestSubstring('longestsubstring') // 8 -> "ubstring" 
findLongestSubstring('thisishowwedoit')  // 6 -> "wedoit"
```
