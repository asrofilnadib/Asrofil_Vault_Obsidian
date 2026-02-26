![[Sederhana tapi buat user tenang.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Data Backdrop Static|Data Backdrop Static]]
- [[#Contoh Codenya|Contoh Codenya]]

## Introduction
Sederhana tapi buat user tenang. Ini case dimana user diharuskan untuk menambahkan data melalui modal. kalau 2 data seperti gambar diatas trus kepencet sembarang, otomatis modal nya hilang tuh. its okey kl masih 2 data, bagaimana kalau 5 data lebih? User sudah keburu males.

Nah supaya tidak terjadi case diatas, kita bisa menggunakan attribut _data-backdrop="static"_.

## Data Backdrop Static
`data-backdrop="static"` di modal Bootstrap digunakan untuk membuat backdrop (area gelap di sekitar modal) menjadi statis, artinya **pengguna tidak bisa menutup modal dengan mengklik di luar modal (di area backdrop)**.

Biasanya, jika `data-backdrop` diatur ke nilai default (`true`), modal akan tertutup jika pengguna mengklik di luar modal. Namun, dengan `data-backdrop="static"`, modal hanya bisa ditutup dengan menekan tombol close atau dengan cara lain yang disediakan dalam modal itu sendiri.

Ini sering digunakan dalam situasi di mana Anda ingin memastikan pengguna tidak bisa menutup modal secara tidak sengaja dan harus menyelesaikan interaksi yang diperlukan di dalam modal terlebih dahulu.

## Contoh Codenya
```html
<div class="modal" id="modal" role="dialog" data-backdrop="static">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="titleModal"></h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <i aria-hidden="true" class="ki ki-close"></i>
                </button>
            </div>
            <div class="modal-body">
                <div id="contentModal"></div>
            </div>
        </div>
    </div>
</div>
```

Note: jika menggunakan bootstrap 5 maka tambahkan bs `data-bs-backdrop="static"`

Date: 12-08-2024