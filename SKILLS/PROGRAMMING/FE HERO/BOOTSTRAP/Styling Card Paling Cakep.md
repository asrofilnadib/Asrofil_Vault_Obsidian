![[Pasted image 20250728140217.png]]
#Tech 
## Introduction
Gue coba iseng-iseng untuk styling card ternyata bagus juga. Oke langsung aja gue kasih kode nya.
## Implementasinya
```html
@extends('layouts.base.oee-report')

@section('content')
<div class="container-fluid mt-4">
    <!-- Welcome Banner -->
    <div class="row">
        <div class="col-12 mb-4">
            <div class="card bg-gradient-primary border-0 shadow-lg">
                <div class="card-body py-4">
                    <div class="row align-items-center">
                        <div class="col-lg-8">
                            <div class="text-center text-lg-left mb-3 mb-lg-0">
                                <h1 class="text-white mb-2">
                                    <i class="fas fa-hand-wave mr-3"></i>
                                    Halo, {{ explode(' ', auth()->user()->name)[0] }}! 👋
                                </h1>
                                <p class="text-white-75 mb-1 lead">
                                    <i class="fas fa-chart-line mr-2"></i>
                                    Selamat datang di Sistem Monitoring OEE Report
                                </p>
                                <p class="text-white-75 mb-0">
                                    <i class="fas fa-building mr-2"></i>
                                    PT. Prakarsa Alam Segar
                                </p>
                            </div>
                        </div>
                        <div class="col-lg-4">
                            <div class="text-center text-lg-right">
                                <div class="bg-white rounded-circle p-3 d-inline-block mb-2">
                                    <i class="fas fa-user-tie fa-2x text-primary"></i>
                                </div>
                                <div>
                                    <span class="badge bg-white text-primary px-3 py-2">
                                        <i class="fas fa-clock mr-1"></i>
                                        {{ \Carbon\Carbon::now()->isoFormat('dddd, D MMMM YYYY') }}
                                    </span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Quick Guide -->
    <div class="row">
        <div class="col-xl-8 col-md-6 mb-4">
            <div class="card border-left-success shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-success text-uppercase mb-1">
                                <i class="fas fa-lightbulb mr-2"></i>Mulai dengan Mudah
                            </div>
                            <h5 class="mb-3">Cara Menggunakan Sistem OEE Report</h5>
                            <div class="row">
                                <div class="col-12">
                                    <ul class="list-unstyled mb-0">
                                        <li class="py-2">
                                            <div class="d-flex align-items-center">
                                                <div class="bg-primary rounded-circle p-2 mr-3">
                                                    <span class="text-white font-weight-bold">1</span>
                                                </div>
                                                <div>
                                                    <strong>Upload Report</strong> - Unggah file laporan OEE harian Anda
                                                </div>
                                            </div>
                                        </li>
                                        <li class="py-2">
                                            <div class="d-flex align-items-center">
                                                <div class="bg-warning rounded-circle p-2 mr-3">
                                                    <span class="text-white font-weight-bold">2</span>
                                                </div>
                                                <div>
                                                    <strong>Template OEE</strong> - Input hasil OEE (Produksi)
                                                </div>
                                            </div>
                                        </li>
                                        <li class="py-2">
                                            <div class="d-flex align-items-center">
                                                <div class="bg-success rounded-circle p-2 mr-3">
                                                    <span class="text-white font-weight-bold">3</span>
                                                </div>
                                                <div>
                                                    <strong>Report</strong> - Lihat dan analisis laporan OEE secara real-time
                                                </div>
                                            </div>
                                        </li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Quick Stats -->
        <div class="col-xl-4 col-md-6 mb-4">
            <div class="card border-left-info shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-info text-uppercase mb-1">
                                <i class="fas fa-tachometer-alt mr-2"></i>Overview Hari Ini
                            </div>
                            <div class="mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span class="text-muted">Noodle1</span>
                                    <span class="font-weight-bold">24</span>
                                </div>
                                <div class="progress rounded-pill" style="height: 8px;">
                                    <div class="progress-bar bg-success" role="progressbar" style="width: 100%"></div>
                                </div>
                            </div>
                            
                            <div class="mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span class="text-muted">Noodle2</span>
                                    <span class="font-weight-bold">18</span>
                                </div>
                                <div class="progress rounded-pill" style="height: 8px;">
                                    <div class="progress-bar bg-primary" role="progressbar" style="width: 75%"></div>
                                </div>
                            </div>
                            
                            <div class="mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span class="text-muted">SEASONING1</span>
                                    <span class="font-weight-bold">20</span>
                                </div>
                                <div class="progress rounded-pill" style="height: 8px;">
                                    <div class="progress-bar bg-warning" role="progressbar" style="width: 85%"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<style>
    .bg-gradient-primary {
        background: linear-gradient(135deg, #4e73df 0%, #224abe 100%) !important;
    }
    
    .bg-gradient-danger {
        background: linear-gradient(135deg, #dc3545 0%, #a71d2a 100%) !important;
    }
    
    .text-white-75 {
        color: rgba(255, 255, 255, 0.75) !important;
    }
    
    .card {
        transition: transform 0.3s ease, box-shadow 0.3s ease;
        border-radius: 0.75rem;
    }
    
    .card:hover {
        transform: translateY(-3px);
        box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15) !important;
    }
    
    .border-left-success {
        border-left: 0.25rem solid #1cc88a !important;
    }
    
    .border-left-info {
        border-left: 0.25rem solid #36b9cc !important;
    }
    
    .text-xs {
        font-size: .75rem;
    }
    
    .font-weight-bold {
        font-weight: 600 !important;
    }
    
    .progress {
        background-color: #e9ecef;
    }
    
    .rounded-pill {
        border-radius: 50rem !important;
    }
    
    .lead {
        font-size: 1.1rem;
        font-weight: 400;
    }
    
    .list-unstyled li {
        border-bottom: 1px solid rgba(0, 0, 0, 0.05);
    }
    
    .list-unstyled li:last-child {
        border-bottom: none;
    }
</style>
@endsection
```
Penjelasan:
- `linear-gradient(135deg, #dc3545 0%, #a71d2a 100%) !important`: membuat gradien linear diagonal dari merah terang (#dc3545) di sudut kiri atas menjadi merah tua (#a71d2a) di sudut kanan bawah dengan sudut 135 derajat.
- `align-items-center`: Membuat semua elemen dalam container flexbox menjadi rata tengah secara vertikal
- `border-left-success`: Menambahkan border kiri berwarna hijau sukses pada elemen
- `font-weight-bold`: Membuat teks menjadi tebal (bold) dengan ketebalan font 700
- `d-flex justify-content-between`: Membuat container flexbox dengan elemen-elemen di dalamnya tersebar merata dengan jarak maksimal di antara mereka (pertama di kiri, terakhir di kanan, sisanya merata di tengah)

Date: 28-07-2025