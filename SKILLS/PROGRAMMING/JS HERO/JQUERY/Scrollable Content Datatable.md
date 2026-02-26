![[Pasted image 20251006160400.png]]
#Tech 
## Introduction
Ada case dimana user itu kesulitan untuk melihat data karena header ngga di freeze saking banyak nya data yg masuk.

## Implementasi
```javascript
function table(){
    $("#table").DataTable({
        bDestroy: true,
        ordering: false,
        width: "100%",
        scrollY: "350px",
        scrollCollapse: true,
        ajax: {...},
        columns: [...]
    });
} table();
```

Penjelasan:
- **scrollY**: berapa tinggi table yg dapat di scroll kebawah.
- **scrollCollapse**: mengaktifkan scroll.
- **width**: memastikan width nya menggunakan persen.

Note: jika ada case dimana header akan terpotong, bungkus datatable pakai function lalu panggil kembali.

Date: 06-10-2025