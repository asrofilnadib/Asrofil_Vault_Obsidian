![[ttd di canvas.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Load CSS dan JS|Load CSS dan JS]]
- [[#JS nya|JS nya]]
- [[#CSS nya|CSS nya]]
- [[#Blade nya|Blade nya]]
- [[#Logic JS nya|Logic JS nya]]
- [[#Logic PHP nya|Logic PHP nya]]

## Introduction
Studi kasus kali ini saya disuruh buat tanda tangan digital menggunakan canvas alias langsung dari web tanpa upload foto.

Source Code : [https://codepen.io/yguo/pen/OyYGxQ](https://codepen.io/yguo/pen/OyYGxQ)

## Load CSS dan JS
```html
<link href="{{ asset('assets/css/signature-pad.css') }}" rel="stylesheet" type="text/css" />
<script src="{{ asset('assets/js/signature-pad.js') }}"></script>
```

## JS nya
```javascript
(function() {
    window.requestAnimFrame = (function(callback) {
      return window.requestAnimationFrame ||
        window.webkitRequestAnimationFrame ||
        window.mozRequestAnimationFrame ||
        window.oRequestAnimationFrame ||
        window.msRequestAnimaitonFrame ||
        function(callback) {
          window.setTimeout(callback, 1000 / 60);
        };
    })();

    var canvas = document.getElementById("sig-canvas");
    var ctx = canvas.getContext("2d");
    ctx.strokeStyle = "#222222";
    ctx.lineWidth = 4;

    var drawing = false;
    var mousePos = {
      x: 0,
      y: 0
    };
    var lastPos = mousePos;

    canvas.addEventListener("mousedown", function(e) {
      drawing = true;
      lastPos = getMousePos(canvas, e);
    }, false);

    canvas.addEventListener("mouseup", function(e) {
      drawing = false;
    }, false);

    canvas.addEventListener("mousemove", function(e) {
      mousePos = getMousePos(canvas, e);
    }, false);

    // Add touch event support for mobile
    canvas.addEventListener("touchstart", function(e) {

    }, false);

    canvas.addEventListener("touchmove", function(e) {
      var touch = e.touches[0];
      var me = new MouseEvent("mousemove", {
        clientX: touch.clientX,
        clientY: touch.clientY
      });
      canvas.dispatchEvent(me);
    }, false);

    canvas.addEventListener("touchstart", function(e) {
      mousePos = getTouchPos(canvas, e);
      var touch = e.touches[0];
      var me = new MouseEvent("mousedown", {
        clientX: touch.clientX,
        clientY: touch.clientY
      });
      canvas.dispatchEvent(me);
    }, false);

    canvas.addEventListener("touchend", function(e) {
      var me = new MouseEvent("mouseup", {});
      canvas.dispatchEvent(me);
    }, false);

    function getMousePos(canvasDom, mouseEvent) {
      var rect = canvasDom.getBoundingClientRect();
      return {
        x: mouseEvent.clientX - rect.left,
        y: mouseEvent.clientY - rect.top
      }
    }

    function getTouchPos(canvasDom, touchEvent) {
      var rect = canvasDom.getBoundingClientRect();
      return {
        x: touchEvent.touches[0].clientX - rect.left,
        y: touchEvent.touches[0].clientY - rect.top
      }
    }

    function renderCanvas() {
      if (drawing) {
        ctx.moveTo(lastPos.x, lastPos.y);
        ctx.lineTo(mousePos.x, mousePos.y);
        ctx.stroke();
        lastPos = mousePos;
      }
    }

    // Prevent scrolling when touching the canvas
    document.body.addEventListener("touchstart", function(e) {
      if (e.target == canvas) {
        e.preventDefault();
      }
    }, false);
    document.body.addEventListener("touchend", function(e) {
      if (e.target == canvas) {
        e.preventDefault();
      }
    }, false);
    document.body.addEventListener("touchmove", function(e) {
      if (e.target == canvas) {
        e.preventDefault();
      }
    }, false);

    (function drawLoop() {
      requestAnimFrame(drawLoop);
      renderCanvas();
    })();

    function clearCanvas() {
      canvas.width = canvas.width;
    }

    // Set up the UI
    var sigText = document.getElementById("sig-dataUrl");
    var sigImage = document.getElementById("sig-image");
    var clearBtn = document.getElementById("sig-clearBtn");
    var submitBtn = document.getElementById("sig-submitBtn");
    clearBtn.addEventListener("click", function(e) {
      clearCanvas();
      sigText.innerHTML = "Data URL for your signature will go here!";
      sigImage.setAttribute("src", "");
    }, false);
    submitBtn.addEventListener("click", function(e) {
      var dataUrl = canvas.toDataURL();
      sigText.innerHTML = dataUrl;
      sigImage.setAttribute("src", dataUrl);
    }, false);

  })();
```

## CSS nya
```css
#sig-canvas {
	border: 2px dotted #CCCCCC;
	border-radius: 15px;
	cursor: crosshair;
}
```

Note : JS dan CSS cukup copas aja dari source code.

## Blade nya
```html
<div class="modal-body">
	<div class="img-container">
		<div class="row">
			<div class="col-md-8">
				<canvas id="sig-canvas" width="620" height="160"></canvas>
				<textarea id="sig-dataUrl" class="form-control" rows="5" style="display: none"></textarea>
				<p>Hasilnya : </p>
				<img id="sig-image" src=""/>
			</div>
		</div>
	</div>
</div>
```

Disini saya akan load canvas, url data nya dan contoh image preview ttd nya!

## Logic JS nya
```javascript
$('#btnSubmit').click(function(){
	var canvas = document.getElementById('sig-canvas');
	var dataUrl = canvas.toDataURL();

	$.ajax({
		url: '{{ route('master.user.signature_ttd_online') }}',
		method:'PUT',
		data:{
			signature_image: dataUrl
		},
		success:function(data)
		{
			$("#ttdDigital").modal("hide");
			window.location.reload()

			Swal.fire({
				icon: 'success',
				title: 'Berhasil',
				text: 'Tanda tangan berhasil disimpan'
			});
		},
		error: function(xhr, status, error) {
			console.error(xhr.responseText);
		}
	});
});
```

Penjelasan :
- Metode `toDataURL()` pada elemen `<canvas>` dalam JavaScript digunakan untuk mengembalikan representasi dalam bentuk URL data dari gambar yang dirender di dalam kanvas. Ini _berguna untuk menyimpan gambar yang telah dibuat atau mengirimkannya ke server._

## Logic PHP nya
```php
public function signature_ttd_online(Request $req)
{
	$picture = $req->signature_image;

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

	// // Store to public folder
	\\File::put('assets/tanda_tangan/' . $imageName, base64_decode($image));

	return response()->json(['success' => true]);
}
```

Note : jangan lupa penggunaan base64.

Date : 31-05-2024