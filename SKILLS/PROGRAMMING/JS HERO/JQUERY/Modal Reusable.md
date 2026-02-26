#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Modal Utama|Modal Utama]]
- [[#Contoh Create Data|Contoh Create Data]]
- [[#Contoh Edit Data|Contoh Edit Data]]

## Introduction
Intinya kita cukup membuat 1 modal namun bisa digunakan untuk create maupun edit atau show data. Jadi tidak terlalu banyak modal.

## Modal Utama
```html
<div class="modal fade" id="templateModal" tabindex="-1" role="dialog" aria-labelledby="modal-fadein" aria-hidden="true">
  <div class="modal-dialog modal-xl" role="document">
      <div class="modal-content">
          <div class="block block-rounded shadow-none mb-0">
              <div class="block-header block-header-default">
              <h3 class="block-title" id="titleModal"></h3>
              <div class="block-options">
                  <button type="button" class="btn-block-option" data-bs-dismiss="modal" aria-label="Close">
                  <i class="fa fa-times"></i>
                  </button>
              </div>
              </div>
              <div class="block-content fs-sm">
                  <div class="row mb-5">
                      <div class="col-lg-10 mx-auto">
                          <div id="containerModal"></div>
                      </div>
                  </div>
              </div>
          </div>
      </div>
  </div>
</div>
```

Penjelasan :
1. `<h3 class="block-title" id="titleModal"></h3>` ini kita buat judul yg dinamis.
2. `<div id="containerModal"></div>` untuk isi form nya yg bisa dinamis.

## Contoh Create Data
```javascript
let statusModal = '';

$("#btnStore").click(function(){
  statusModal = 'Create Data';
  $("#titleModal").text(statusModal);

  $("#containerModal").empty();

  $("#containerModal").append(`
    <div class="mt-1">
      <label>Judul</label>
      <input type="text" class="form-control" id="title" required>
    </div>
    <div class="mt-2">
      <button class="btn btn-primary" id="btnSubmit">Submit</button>
      <button class="btn btn-warning" onClick="cancelSubmit()">Cancel</button>
    </div>
  `);
  $("#templateModal").modal("show");
});
```

## Contoh Edit Data
```javascript
$(document).on("click", ".btnShow", function(){
  statusModal = "Show Template";

  let id = $(this).data("id");
  $("#titleModal").html(statusModal);

  $("#containerModal").empty();

  $.ajax({
    type: "GET",
    url: "/ba-coproduct/masters/template/" + id,
    success: function(data){
      $("#templateModal").modal("show");
      $("#containerModal").append(data.content);
    },
    error: function(xhr){
      console.log(xhr.responseText)
    }
  });
});
```

Date : 29-05-2024