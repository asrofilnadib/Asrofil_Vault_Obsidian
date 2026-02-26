![[Crop gambar sebelum upload.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Load css dan js|Load css dan js]]
- [[#Blade nya|Blade nya]]
- [[#Logic JS nya|Logic JS nya]]
- [[#Logic PHP nya|Logic PHP nya]]

## Introduction
Kali ini saya dapat studi kasus crop tanda tangan sebelum upload. Kali ini saya menggunakan cropper js, linknya [disini](https://fengyuanchen.github.io/cropperjs/)

## Load css dan js
```html
<link href="{{ asset('assets/css/cropper.css') }}" rel="stylesheet" type="text/css" />
<script src="{{ asset('assets/js/cropper.js') }}"></script>
```

## Blade nya
```html
<label for="upload_image" class="btn btn-primary">
	Upload Tanda Tangan
	<input accept="image/*" type="file" id="upload_image" style="display: none">
</label>

<div class="modal fade" id="modal" tabindex="-1" role="dialog" aria-labelledby="modalLabel" aria-hidden="true">
    <div class="modal-dialog modal-lg" role="document">
      <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Crop Image Before Upload</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
              </button>
            </div>
            <div class="modal-body">
              <div class="img-container">
                  <div class="row">
                      <div class="col-md-8">
                          <img src="" id="sample_image" style="max-width: 100%" />
                      </div>
                      <div class="col-md-4">
                          <div class="border preview_image"></div>
                      </div>
                  </div>
              </div>
            </div>
            <div class="modal-footer">
                <button type="button" id="crop" class="btn btn-primary">Crop</button>
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
            </div>
      </div>
    </div>
</div>
```

## Logic JS nya
```javascript
var $modal = $('#modal');
    var image = document.getElementById('sample_image');
    var cropper;

    $('#upload_image').change(function(event){
        var files = event.target.files;

        var done = function(url){
            image.src = url;
            $modal.modal('show');
        };

        if(files && files.length > 0)
        {
            reader = new FileReader();
            reader.onload = function(event)
            {
                done(reader.result);
            };
            reader.readAsDataURL(files[0]);
        }
    });

    $modal.on('shown.bs.modal', function() {
        cropper = new Cropper(image, {
            // aspectRatio: 1.4,
            // viewMode: 3,
            preview:'.preview_image',
        });
    }).on('hidden.bs.modal', function(){
        cropper.destroy();
            cropper = null;
    });

    $('#crop').click(function(){
        canvas = cropper.getCroppedCanvas({
            width:400,
            height:400
        });

        canvas.toBlob(function(blob){
            url = URL.createObjectURL(blob);
            var reader = new FileReader();
            reader.readAsDataURL(blob);
            reader.onloadend = function(){

                Swal.fire({
                    title: 'Mohon tunggu',
                    text: 'Tanda tangan sedang diproses',
                    allowOutsideClick: false,
                    allowEscapeKey: false,
                    onBeforeOpen: function () {
                        Swal.showLoading();
                        console.log('testing...')
                    }
                })

                var base64data = reader.result;
                $.ajax({
                    url: '{{ route('master.user.signature_upd') }}',
                    method:'PUT',
                    data:{
                        tanda_tangan:base64data
                    },
                    success:function(data)
                    {
                        $modal.modal('hide');

                        window.location.reload()

                        Swal.fire({
                            icon: 'success',
                            title: 'Berhasil',
                            text: 'Tanda tangan berhasil disimpan'
                        })
                        // $('#uploaded_image').attr('src', data);
                    }
                });
            };
        });
    });
```

Note : perhatikan di baris awal. Uniknya di dalam label pakai tag input. Penjelasan :
- `event.target.files` properti yang digunakan dalam pemrograman web, terutama dalam pengembangan aplikasi web yang melibatkan pengunggahan file
- `FileReader()` digunakan untuk membaca konten dari berkas (file) secara asinkron.

## Logic PHP nya
```php
public function signature_upd(Request $req)
{
	$picture = $req->tanda_tangan;

	if(explode('data:image/png', $picture) > 1) {
		// Maka dia png
		$image = str_replace('data:image/png;base64,', '', $picture);
		$extension = '.png';
	}else{
		// Maka dia jpg
		$image = str_replace('data:image/jpeg;base64,', '', $picture);
		$extension = '.jpg';
	}

	$imageName = uniqid() . $extension;
	$image = str_replace(' ', '+', $image);

	User::where('id', Auth::user()->id)
			->update([
				'tanda_tangan' => $imageName
			]);

	$mask = Image::make($image);
	$mask->greyscale();
	$mask->contrast(100);
	$mask->contrast(50);
	$mask->trim('top-left', null, 40);
	$mask->invert();

	$new_image = Image::canvas($mask->width(), $mask->height(), '#000000')
	->mask($mask);

	$new_image->save('assets/tanda_tangan/' . $imageName, base64_decode($image));

	return response()->json([
		'success' => '1',
		'message' => 'File uploaded'
	]);
}
```

Intinya kalau ada penggunaan upload gambar menggunakan plugins atau ajax itu akan jauh berbeda codingannya dengan upload gambar biasa.

Date : 31-05-2024