#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kode nya|Kode nya]]
- [[#Screenshot|Screenshot]]

## Introduction
Ketika gue mendapatkan project CPAR Audit Internal Eksternal ini gue dapet cara bagaimana menampilkan pdf tanpa new tab. Ataupun menampilkan gambar tanpa lightbox.js. Yaps pakai Sweetalert2

## Kode nya
```javascript
${res.picture.map(picture => {
    if (picture.jenis === 'ca_pa') {
        if (picture.picture.endsWith('.pdf')){
            return `
            <li>
                <a href="#" data-pdf="/storage/cpar-audit/internal/foto/${picture.picture}" class="view-pdf" style="margin-bottom: 10px;">
                    ${picture.picture}
                </a>
            </li>
        `;
        }else{
            return `
            <li>
                <a href="#" data-image="/storage/cpar-audit/internal/foto/${picture.picture}" class="view-image" style="margin-bottom: 10px;">
                    ${picture.picture}
                </a>
            </li>
            `;
        }
    } 

    return ''; 
}).join('')}
```

```javascript
$('.view-image').on('click', function(e) {
      e.preventDefault();
      const imageUrl = $(this).data('image'); 

      Swal.fire({
          imageUrl: imageUrl,
          showCloseButton: true,
          showCancelButton: false,
          confirmButtonText: 'Download',
          customClass: {
              popup: 'swal2-popup',
          },
          preConfirm: () => {
              const link = document.createElement('a');
              link.href = imageUrl; 
              link.download = ''; 
              document.body.appendChild(link);
              link.click(); 
              document.body.removeChild(link); 
          }
      });
  });

  $('.view-pdf').on('click', function(e) {
      e.preventDefault();
      const pdfUrl = $(this).data('pdf'); 

      Swal.fire({
          title: 'Lihat PDF',
          html: `
              <iframe src="${pdfUrl}" style="width: 100%; height: 400px;" frameborder="0"></iframe>
              <a href="${pdfUrl}" download class="btn btn-primary" style="margin-top: 10px;">Download PDF</a>
          `,
          showCloseButton: true,
          showCancelButton: false,
          customClass: {
              popup: 'swal2-popup',
          },
          focusConfirm: false,
          onOpen: () => {
          }
      });
  });
```

Penjelasan:
- **`imageUrl: imageUrl`**: Menampilkan gambar dalam modal dengan URL yang diambil dari `data-image`.
- **`showCloseButton: true`**: Menampilkan tombol close (X) di pojok kanan atas modal.
- **`showCancelButton: false`**: Menyembunyikan tombol cancel.
- **`confirmButtonText: 'Download'`**: Mengubah teks tombol konfirmasi menjadi "Download".
- **`customClass: { popup: 'swal2-popup' }`**: Menambahkan kelas kustom ke modal.
- **`preConfirm: () => { ... }`**: Fungsi yang dijalankan ketika tombol "Download" diklik.
    - **`const link = document.createElement('a');`**: Membuat elemen `<a>` baru.
    - **`link.href = imageUrl;`**: Mengatur `href` dari elemen `<a>` ke URL gambar.
    - **`link.download = '';`**: Mengatur atribut `download` untuk memungkinkan pengunduhan gambar.
    - **`document.body.appendChild(link);`**: Menambahkan elemen `<a>` ke dalam body dokumen.
    - **`link.click();`**: Memicu klik pada elemen `<a>` untuk memulai pengunduhan.
    - **`document.body.removeChild(link);`**: Menghapus elemen `<a>` dari body dokumen setelah pengunduhan.
- **`const pdfUrl = $(this).data('pdf');`**: Mengambil nilai dari atribut `data-pdf` pada elemen yang diklik. Ini biasanya berisi URL PDF yang akan ditampilkan.
- **`Swal.fire({ ... })`**: Membuka modal SweetAlert2 dengan konfigurasi tertentu.
    - **`title: 'Lihat PDF'`**: Menetapkan judul modal.
    - **`html: ...`**: Menentukan konten HTML yang akan ditampilkan dalam modal.
        - **`<iframe src="${pdfUrl}" style="width: 100%; height: 400px;" frameborder="0"></iframe>`**: Menampilkan PDF dalam iframe dengan lebar 100% dan tinggi 400px.
        - **`<a href="${pdfUrl}" download class="btn btn-primary" style="margin-top: 10px;">Download PDF</a>`**: Menambahkan tombol untuk mengunduh PDF.
    - **`showCloseButton: true`**: Menampilkan tombol close (X) di pojok kanan atas modal.
    - **`showCancelButton: false`**: Menyembunyikan tombol cancel.
    - **`customClass: { popup: 'swal2-popup' }`**: Menambahkan kelas kustom ke modal.
    - **`focusConfirm: false`**: Mencegah fokus otomatis pada tombol konfirmasi.
    - **`onOpen: () => { ... }`**: Fungsi yang dijalankan saat modal dibuka (dalam kasus ini, tidak ada tindakan khusus).

## Screenshot
![[Screenshot_278.png]]

![[Screenshot_279.png]]

Date: 04-02-2025