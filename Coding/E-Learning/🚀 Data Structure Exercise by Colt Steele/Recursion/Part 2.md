# 📘 Recursion Challenge Section – Problem Set

---

### 1️⃣ **Reverse Solution**

**Problem Statement:**  
Buatlah fungsi `reverse(str)` yang menerima sebuah string dan mengembalikan string tersebut dalam urutan terbalik menggunakan **rekursi** (bukan built-in `reverse()`).

#### Contoh:
```javascript
reverse("awesome")    // "emosewa" 
reverse("rithmschool") // "loohcsmhtir"
```
---
### 2️⃣ **isPalindrome Solution**

**Problem Statement:**  
Buatlah fungsi `isPalindrome(str)` yang memeriksa apakah string yang diberikan adalah **palindrome**.  
Palindrome adalah kata/kalimat yang sama jika dibaca dari depan maupun belakang.

#### Contoh:
```javascript
isPalindrome("awesome")   // false 
isPalindrome("foobar")    // false 
isPalindrome("tacocat")   // true 
isPalindrome("amanaplanacanalpanama") // true
```
---
### 3️⃣ **someRecursive Solution**

**Problem Statement:**  
Buatlah fungsi `someRecursive(array, callback)` yang menerima array dan sebuah callback.  
Fungsi harus mengembalikan `true` jika **setidaknya satu elemen array** memenuhi kondisi dari callback, dan `false` jika tidak ada satupun yang memenuhi.

#### Contoh Callback:
```javascript
// Callback untuk cek bilangan ganjil const isOdd = val => val % 2 !== 0;
```

#### Contoh:
```javascript
someRecursive([1,2,3,4], isOdd)   // true   (karena ada angka ganjil) 
someRecursive([4,6,8,9], isOdd)   // true   (karena 9 ganjil) 
someRecursive([4,6,8], isOdd)     // false  (semua genap)
```
---
### 4️⃣ **Flatten Solution**

**Problem Statement:**  
Buatlah fungsi `flatten(oldArr)` yang menerima array yang mungkin berisi nested array (array di dalam array), lalu mengembalikan array baru yang **datar (1 dimensi)**. Gunakan rekursi.

#### Contoh:
```javascript
flatten([1, 2, 3, [4, 5]])  // [1, 2, 3, 4, 5]  
flatten([1, [2, [3, 4], [[5]]]])  // [1, 2, 3, 4, 5]  
flatten([[1],[2],[3]])  // [1,2,3]  
flatten([[[[1], [[[2]]], [[[[[[[3]]]]]]]]]])  // [1,2,3]
```
