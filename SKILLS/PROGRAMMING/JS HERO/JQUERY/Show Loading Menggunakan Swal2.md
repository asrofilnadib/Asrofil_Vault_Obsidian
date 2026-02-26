![[Show Loading Menggunakan SWAL2.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Implementasi|Implementasi]]

## Introduction
Sederhana tapi buat user seneng. Bagaimana jika sedang loading tapi tidak ditampilkan? user pasti akan klik upload lagi. jadi ada kemungkinan ada data double. Nah dengan memunculkan notifikasi loading membuat user tau bahwa sistem sedang loading.

## Implementasi
```javascript
function uploadNonActive(){
    let excelFile = $("#fileUpload")[0].files[0];
    let formData = new FormData();
    formData.append('excel_file', excelFile);

    Swal.fire({
        title: 'Mohon menunggu...',
        text: 'Sedang mengunggah data...',
        allowOutsideClick: false,
        onBeforeOpen: () => {
            Swal.showLoading();
        }
    });

    $.ajax({
        type: "POST",
        url: "/noodle_1/material/master/uploadNonActive",
        data: formData,
        processData: false,
        contentType: false,
        success: function(res) {
            Swal.close(); // jangan lupa di close

            table(search, variant, jenis_material);
            notify("success", "success", res.message);
            $("#modal").modal("hide");
        },
        error: function(xhr) {
            Swal.close();
            let message = xhr.responseJSON.message || 'Terjadi kesalahan saat mengunggah file.';
            alert(message);
        }
    });
}
```

Date: 12-10-2024