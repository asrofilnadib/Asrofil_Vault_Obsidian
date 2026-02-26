![[widget-wa.png]]
#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
Kadang kala ketika kita membuat website, pengguna membutuhkan tombol wa widget untuk mempermudah dia menghubungi kita melalui whatsapp. Berikut codingannya!

Index.html
```html
<a href="<https://wa.me/>{{ $user->phone_number }}" target=”_blank” class="whatsapp-btn">
	<i class="bi bi-whatsapp"></i>
</a>

```

Note : Letakkan di bawah footer atau diatas footer yang ada.

style.css
```css
.whatsapp-btn {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 9999;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background-color: #25D366;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
    animation: breathe 2s ease-in-out infinite;
}

.whatsapp-btn i {
    color: #fff;
    font-size: 24px;
    animation: beat 2s ease-in-out infinite;
    text-decoration: none;
}

@keyframes breathe {
    0% {
      box-shadow: 0 0 0 0 rgba(37, 211, 102, 0.5);
    }

    70% {
      box-shadow: 0 0 0 15px rgba(37, 211, 102, 0);
    }

    100% {
      box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
    }
  }

@keyframes beat {
    0% {
      transform: scale(1);
    }

    50% {
      transform: scale(1.2);
    }

    100% {
      transform: scale(1);
    }
}
```