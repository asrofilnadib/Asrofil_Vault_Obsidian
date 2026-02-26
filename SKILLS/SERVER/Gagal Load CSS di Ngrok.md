#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
Saya mengalami ini ketika develop website CBT SMAS Yappenda. Awalnya saya menggunakan hosting, karena ada problem ketika mengakses storage, akhirnya saya menggunakan ngrok. Namun disatu sisi ketika itu saya tidak bisa load css? padahal di local aman-aman saja.

Setelah dicari penyebabnya ternyata cukup mudah, yaitu cukup menambahkan tag dibawah pada root file kita.

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

Penjelasan :
- sebuah tag meta yang digunakan untuk mengatur kebijakan keamanan konten (Content Security Policy, CSP) pada sebuah halaman web
- `upgrade-insecure-requests` adalah sebuah direktif kebijakan CSP yang menginstruksikan browser untuk mengupgrade permintaan (requests) yang tidak aman (misalnya HTTP) menjadi aman (misalnya HTTPS). Hal ini membantu dalam meningkatkan keamanan halaman web dengan memastikan bahwa sumber daya yang dimuat menggunakan protokol yang aman.

Date : 07-03-2024