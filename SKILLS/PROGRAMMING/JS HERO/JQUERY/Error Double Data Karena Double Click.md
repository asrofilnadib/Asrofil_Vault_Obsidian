#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Apa Itu Flag?|Apa Itu Flag?]]
- [[#Contoh Penggunaan Flag|Contoh Penggunaan Flag]]

## Introduction
Ini terjadi di project QA Lab. Dimana user melakukan klik beberapa kali ketika jaringan lemot. Ini menyebabkan permintaan ganda ke server. Ini dapat terjadi jika pengguna mengklik tombol "Simpan" beberapa kali sebelum respons dari server diterima. Kali ini gue memilih menggunakan flag untuk mencegah pengiriman data.

## Apa Itu Flag?
- **Definisi**: Flag adalah variabel yang digunakan untuk menunjukkan kondisi atau status tertentu dalam program. Biasanya, flag diwakili sebagai nilai boolean (true/false).
- **Fungsi**: Flag berfungsi untuk mengontrol alur program dengan memberikan sinyal kapan kondisi tertentu terpenuhi atau tindakan harus diambil.

## Contoh Penggunaan Flag
```javascript
let isSubmitting = false;

$(document).on("click", "#submitBtn", function() {
    if (isSubmitting) return; // Jika sudah dalam proses, keluar dari fungsi
    isSubmitting = true; // Set flag menjadi true

    let data = [];
    
    $('#table tbody tr').each(function() {
        let rowIndex = $(this).attr('id').split('-')[1]; 
        let rowData = {
            mid: $(`#mid_${rowIndex}`).val(),
            kategori: $(`#kategori_${rowIndex}`).val(),
            tgl_produksi: $(`#tgl_produksi_${rowIndex}`).val(),
            batch: $(`#batch_${rowIndex}`).val(),
            jumlah_sampel: $(`#jumlah_sampel_${rowIndex}`).val(),
        };
        
        if (rowData.mid 
            && rowData.tgl_produksi 
            && rowData.batch 
            && rowData.jumlah_sampel 
        ) {
            data.push(rowData);
        }
    });
    
    $.ajax({
        url: "/shelf-life-qa/qc/seasoning/tambah-sample-sambal", 
        method: "POST",
        data: {
            data: data  
        },
        success: function(res) {
            if (res.success) {
                notification("Berhasil","success","Data berhasil disimpan.");
                resetForm();
            } 
            
            if(res.varian_validation){
                notification("Gagal","error",res.message);
            }
        },
        error: function(xhr){
            let errors = xhr.responseJSON.errors;
            let message = '';
            $.each(errors, function(key, value) {
                message += value[0] + '<br>';
            });

            notification("Error","error",message);
        },
        complete: function() {
            isSubmitting = false; // Reset flag setelah permintaan selesai
        }
    });
});
```

Date: 28-01-2025