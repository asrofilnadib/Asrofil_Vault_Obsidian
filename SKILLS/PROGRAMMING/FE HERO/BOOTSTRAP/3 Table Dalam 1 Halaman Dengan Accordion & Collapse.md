#Tech 
## Introduction
Gue dapet case menarik dimana dalam 1 halaman akan menampilkan 3 data: data sudah dikonfirmasi, belum dikonfirmasi dan di reject.

## Implementasinya

```html
<div class="accordion" id="accordinButton">
	<a class="bg-primary" data-toggle="collapse" href="#collapseA">
		<i class="fas fa-list text-white"></i> A
	</a>
	<a class="bg-primary" data-toggle="collapse" href="#collapseB">
		<i class="fas fa-list text-white"></i> B
	</a>
	<a class="bg-primary" data-toggle="collapse" href="#collapseC">
		<i class="fas fa-list text-white"></i> C
	</a>
	
	<div id="collapseA" class="collapse show" data-parent="#accordinButton">
		<h1>A</h1>
	</div>
	<div id="collapseB" class="collapse" data-parent="#accordinButton">
		<h1>B</h1>
	</div>
	<div id="collapseC" class="collapse" data-parent="#accordinButton">
		<h1>C</h1>
	</div>
</div>
```

## Hasilnya
![[bandicam 2025-05-01 18-03-35-460.mp4]]

Date: 01-05-2025