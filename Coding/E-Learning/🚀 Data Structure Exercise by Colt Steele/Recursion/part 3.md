# 📘 Recursion Challenge – Problem Set Part 2
---
### 1️⃣ **capitalizeWords Solution**

**Problem Statement:**  
Buatlah fungsi `capitalizeWords(array)` yang menerima array berisi string.  
Fungsi harus mengembalikan array baru dengan semua kata diubah menjadi **huruf kapital penuh**.

#### Contoh:
```javascript
capitalizeWords(['i', 'am', 'learning', 'recursion']) 
// ["I", "AM", "LEARNING", "RECURSION"]
```
---
### 2️⃣ **nestedEvenSum Solution**

**Problem Statement:**  
Buatlah fungsi `nestedEvenSum(obj)` yang menerima sebuah objek yang mungkin berisi objek lain di dalamnya.  
Fungsi harus mengembalikan jumlah semua **angka genap** di dalam objek, termasuk angka genap yang berada di nested object.

#### Contoh:
```javascript
var obj1 = {
  outer: 2,
  obj: {
    inner: 2,
    otherObj: {
      superInner: 2,
      notANumber: true,
      alsoNotANumber: "yup"
    }
  }
}

nestedEvenSum(obj1) 
// 6
```
---
### 3️⃣ **capitalizeFirst Solution**

**Problem Statement:**  
Buatlah fungsi `capitalizeFirst(array)` yang menerima array string.  
Fungsi harus mengembalikan array baru dengan **huruf pertama setiap kata dikapitalisasi**, sedangkan huruf berikutnya tetap.

#### Contoh:
```javascript
capitalizeFirst(['car','taco','banana']) 
// ["Car", "Taco", "Banana"]
```
---
### 4️⃣ **stringifyNumbers Solution**

**Problem Statement:**  
Buatlah fungsi `stringifyNumbers(obj)` yang menerima sebuah objek.  
Fungsi harus mengembalikan objek baru dengan semua nilai angka dikonversi menjadi **string**, sementara nilai selain angka tetap sama.

#### Contoh:
```javascript
let obj = {
  num: 1,
  test: [],
  data: {
    val: 4,
    info: {
      isRight: true,
      random: 66
    }
  }
}

stringifyNumbers(obj)
/* 
{
  num: "1",
  test: [],
  data: {
    val: "4",
    info: {
      isRight: true,
      random: "66"
    }
  }
}
*/
```
---
### 5️⃣ **collectStrings Solution**

**Problem Statement:**  
Buatlah fungsi `collectStrings(obj)` yang menerima sebuah objek yang mungkin berisi nested object.  
Fungsi harus mengembalikan sebuah array berisi semua **string** yang ada di dalam objek tersebut.  
(Implementasi boleh dengan **helper recursion** atau **pure recursion**).

#### Contoh:
```javascript
const obj = {
  stuff: "foo",
  data: {
    val: {
      thing: {
        info: "bar",
        moreInfo: {
          evenMoreInfo: {
            weMadeIt: "baz"
          }
        }
      }
    }
  }
}

collectStrings(obj) 
// ["foo", "bar", "baz"]
```
