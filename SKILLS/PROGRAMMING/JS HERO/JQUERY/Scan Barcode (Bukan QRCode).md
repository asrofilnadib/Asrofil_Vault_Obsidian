<img src="IMG-20250218-WA0010.jpeg.png" width="450" align="left">

















Jenis Barcode: CODE_39
#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Kode nya|Kode nya]]
- [[#Demo|Demo]]

## Introduction
Di project TMS ini ada case unik dimana system gue harus bisa scan barcode. Kalau Scan QR Code sangat memungkinkan. Bagaimana dengan barcode disamping?

Setelah beberapa hari gue nyari tau ternyata gue dapet 1 plugin namanya “QuaggaJS”. Jadi plugin ini digunakan untuk scan berbagai macam barcode.

## Kode nya
```html
<!DOCTYPE html>
<html>
<head>
  <title>Barcode Scanner</title>
  <script src="<https://code.jquery.com/jquery-3.6.0.min.js>"></script>
  <script src="<https://cdnjs.cloudflare.com/ajax/libs/quagga/0.12.1/quagga.min.js>"></script>
  <style>
    #interactive.viewport {
      position: relative;
      width: 640px;
      height: 480px;
      overflow: hidden;
    }

    #interactive canvas.overlay {
      position: absolute;
      top: 0;
      left: 0;
    }
    #barcode-result { /* Style untuk menampilkan hasil */
      margin-top: 10px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div id="interactive" class="viewport"></div>
  <h1 id="barcode-result">Hasil barcode akan ditampilkan di sini</h1> <div id="error-message"></div>

  <script>
    $(document).ready(function() {
      Quagga.init({
        inputStream : {
          name : "Live",
          type : "LiveStream",
          target: document.querySelector('#interactive') // Perbaikan: target harus #interactive
        },
        decoder : {
          readers : [
                "code_39_reader"
            ] 
        }
      }, function(err) {
        if (err) {
          console.error(err); // Gunakan console.error untuk debugging
          $("#error-message").text("Error: " + err.message); // Tampilkan pesan error
          return;
        }
        console.log("Initialization finished. Ready to start");
        Quagga.start();
      });

      Quagga.onDetected(function(data) {
        var code = data.codeResult.code;
        console.log("Barcode detected and decoded: " + code);
        $("#barcode-result").append("Barcode: " + code);

        Quagga.stop(); 
      });

      Quagga.onProcessed(function(result) {
        var drawingCtx = Quagga.canvas.ctx.overlay;
        var lines = result.lines;

        if (lines) {
          lines.forEach(function (line) {
            drawingCtx.strokeStyle = 'red';
            drawingCtx.lineWidth = 3;
            drawingCtx.beginPath();
            drawingCtx.moveTo(line.x0, line.y0);
            drawingCtx.lineTo(line.x1, line.y1);
            drawingCtx.stroke();
          });
        }
      });
    });
  </script>
</body>
</html>
```

## Demo
<video width="640" height="360" controls>
    <source src="bandicam 2025-02-18 13-44-04-166 (1).mp4" type="video/ogg">
</video>