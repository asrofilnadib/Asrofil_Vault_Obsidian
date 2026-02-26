<img src="Cannot reinitialise DataTable.png" width="450" align="left">









#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kodingan|Kodingan]]

## Introduction
Jadi saya ingin membungkus datatable dengan function. dan saya ingin memanggilnya kembali di function lain. Namun terdapat error reinitialise.

## Kodingan
```javascript
function table(){
    $("#masterBankTable").DataTable({
        searching: true,
        serverSide: true,
        info: false,
        paging: false,
        ordering: false,
        ajax: {
            type: "GET",
            url: "/su/master/bank/getData"
        },
        columns: [
            // Kolomnya
        ]
    });
}

function remove(id){
    Swal.fire({
        title: "Konfirmasi",
        icon: "info",
        text: "Yakin ingin menghapus data?",

        showCancelButton: true,
        confirmButtonText: "Hapus",
        confirmButtonColor: "#ED5E5E"
    }).then((result) => {
        if(result.isConfirmed){
            $.ajax({
                type: "DELETE",
                url: "/su/master/bank/remove",
                data: {
                    id
                },
                success: function(res){
                    notifySuccess(res.message);
                    table();
                }
            });
        }
    });
}
```

Solusinya adalah menggunakan `bDestroy: true` dalam konfigurasi DataTables digunakan untuk menghapus _instance datatable_ yang ada sebelum membuat instance baru.

Date: 13-07-2024