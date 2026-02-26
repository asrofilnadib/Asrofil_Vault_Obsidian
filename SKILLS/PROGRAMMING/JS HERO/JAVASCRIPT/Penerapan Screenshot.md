![[Screenshot from 2024-12-10 13-59-07.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kode|Kode]]

## Introduction
Pada case ini gue di challenge untuk mempermudah pekerjaan pak Anang (dept Eng) PRN1. Gue di awal coba pakai datatable dan memanfaatkan fitur export pdf tapi karena menggunakan plugin diluar datatable maka sangat sulit.

Setelah sholat Zuhur gue sadar bahwa "Kenapa ngga gue full screen dan take a shot". Oke itu jalan keluarnya.

## Kode
```html
<div class="modal" id="deptModal" role="dialog" data-backdrop="static">
    <div class="modal-dialog modal-xl" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">Data Mesin untuk Departemen <span id="deptName"></span></h5>
                <button type="button" class="close closeModal" data-dismiss="modal" aria-label="Close">
                    <i aria-hidden="true" class="ki ki-close"></i>
                </button>
            </div>
            <div class="modal-body">
                <button class="btn btn-secondary mb-5" id="toggleFullscreenBtn">Toggle Fullscreen</button>
                <button class="btn btn-primary mb-5" id="screenshotBtn">Ambil Screenshot</button>
                <div class="row" id="containerQRByDept"></div>
            </div>
        </div>
    </div>
</div>

```

```jsx
document.getElementById('screenshotBtn').addEventListener('click', function() {
    html2canvas(document.querySelector("#containerQRByDept")).then(canvas => {
        const link = document.createElement('a');
        link.href = canvas.toDataURL('image/png');
        link.download = 'screenshot.png';
        link.click();
    }).catch(error => {
        console.error('Error capturing screenshot:', error);
    });
});

$(document).ready(function() {
    $('#toggleFullscreenBtn').on('click', function() {
        $('#deptModal').toggleClass('fullscreen-modal');
    });
});

```

Jangan lupa load html2canvas nya

```html
<script src="<https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js>"></script>

```

Date: 10-12-2024