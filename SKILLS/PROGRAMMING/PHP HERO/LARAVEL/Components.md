#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Membuat Component|Membuat Component]]
- [[#Views Alert|Views Alert]]
- [[#Controller Alert|Controller Alert]]

## Introduction
Bayangkan jika kamu memiliki 1 alert yg sama dan banyak digunakan pada banyak template. Hal ini membuat codingan kamu tidak efektif. Bagaimana jika kita membuat 1 alert di hanya 1 wadah dan dapat dipanggil di banyak template?

The Solution is Component!

## Membuat Component
```
php artisan make:component alert
```

secara otomatis akan dibuatkan 1 template dan 1 controller, seperti berikut :
- resources/views/components/alert.blade.php
- app/view/components/alert.php

Sederhananya, kita akan mempassing 2 data ke component alert diawali dengan session.
- type untuk menentukan color apa yg akan kita panggil (cth: primary/biru)
- sess untuk mengirimkan session yg telah kita definisikan sendiri(bebas)

```html
@if(session('success'))
<div class="col-lg-12">
	<x-alert type="primary" sess="success" />
</div>
@endif
```

## Views Alert
```html
<div class="alert alert-{{ $type }} alert-dismissible" role="alert">
    <button type="button" class="close" data-dismiss="alert" aria-label="Close">
        <span aria-hidden="true">&times;</span>
    </button>
    {{ session($sess) }}
</div>
```

## Controller Alert
```php
class Alert extends Component
{
    public $type;
    public $sess;

    public function __construct($type, $sess)
    {
        $this->type = $type;
        $this->sess = $sess;
    }

    public function render()
    {
        return view('components.alert');
    }
}
```

Tidak seperti passing data pada controller (compact), kita tidak perlu mempassing data di komponen (dapat disesuaikan).