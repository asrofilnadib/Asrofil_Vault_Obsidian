### 1️⃣ **Power Solution**

**Problem Statement:**  
Buatlah fungsi `power(base, exponent)` yang menerima dua parameter: angka dasar (`base`) dan pangkat (`exponent`).  
Fungsi harus menghitung nilai `base` dipangkatkan `exponent` tanpa menggunakan operator bawaan `**` atau `Math.pow()`. Gunakan **rekursi**.

Contoh:
```javascript
power(2, 0)   // 1   (karena apapun pangkat 0 hasilnya 1)
power(2, 2)   // 4   (2 * 2)
power(2, 4)   // 16  (2 * 2 * 2 * 2)
```

### 2️⃣ **Factorial Solution**

**Problem Statement:**  
Buatlah fungsi `factorial(x)` yang menghitung **faktorial** dari sebuah bilangan `x`.  
Ingat bahwa:

- `0! = 1`
    
- `n! = n * (n-1)!` untuk n > 0.  
    Jika `x < 0`, kembalikan `0` sebagai nilai default.
    

Contoh:
```javascript
factorial(0)   // 1 
factorial(1)   // 1 
factorial(5)   // 120  (5 * 4 * 3 * 2 * 1)
```

---

### 3️⃣ **Product of Array Solution**

**Problem Statement:**  
Buatlah fungsi `productOfArray(arr)` yang menerima sebuah array angka.  
Fungsi harus mengembalikan hasil perkalian **semua elemen** array menggunakan rekursi.  
Jika array kosong (`[]`), hasilnya `1` (karena identitas perkalian adalah 1).

Contoh:
```javascript
productOfArray([1,2,3])      // 6   (1*2*3) 
productOfArray([1,2,3,10])   // 60  (1*2*3*10) 
productOfArray([])           // 1
```

---

### 4️⃣ **Recursive Range Solution**

**Problem Statement:**  
Buatlah fungsi `recursiveRange(x)` yang menghitung jumlah semua angka dari `0` hingga `x`.  
Gunakan rekursi, bukan loop.

Contoh:
```javascript
recursiveRange(6)   // 21  (6+5+4+3+2+1+0) 
recursiveRange(10)  // 55  (10+9+8+7+6+5+4+3+2+1+0)
```

---

### 5️⃣ **Fibonacci Solution**

**Problem Statement:**  
Buatlah fungsi `fib(n)` yang mengembalikan **angka ke-n** dalam deret Fibonacci.  
Ingat bahwa:

- `fib(1) = 1`, `fib(2) = 1`
    
- `fib(n) = fib(n-1) + fib(n-2)`
    

Contoh:
```javascript
fib(4)   // 3   (urutan: 1, 1, 2, 3) 
fib(10)  // 55  (urutan: 1,1,2,3,5,8,13,21,34,55)
```