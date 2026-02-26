![[Desain Tabel Template.png]]
#Tech 
# Table of Content
- [[#Implementasi|Implementasi]]

## Implementasi
```html
@push('styles')
    <style type="text/css">
        .hide {
            display: none;
        }

        #tableTraceability{
            border-radius: 20px;
            border-collapse: separate;
            overflow: hidden;
            border-spacing: 0;
        }

        .cup_sleeve{
            background-color: #ff8d8f;
            color: white !important;
            text-align: center;
        }

        .lid_cup{
            background-color: #a77bff;
            color: white !important;
            text-align: center;
        }

        .garpu{
            background-color: #4ab1f6;
            color: white !important;
            text-align: center;
        }

        .supplier{
            background-color: #4dff94;
            color: white !important;
            text-align: center;
        }

        .table td {
            text-align: center;
        }

        h6{
            text-align: center;
        }
    </style>
    <link rel="stylesheet" href="{{ url('/assets/plugins/custom/datatables/datatables.bundle.css') }}">
@endpush

@section('content')
<div class="container">
    <div class="main-body">
        <div class="row">
            <div class="col-12">
                <div class="card card-custom mb-4">
                    <div class="card-header">
                        <div class="card-title">
                            <a href="{{ url('/prn2-lp/transaction') }}" class="btn btn-secondary mr-4"><i class="fas fa-chevron-circle-left icon-md"></i> Back</a>
                            <span class="badge badge-primary" style="font-weight:500">NOODLE 2</span>&nbsp;
                            <h3 class="card-label text-dark-50">
                                TRACEABILITY MATERIAL  | <span class="text-dark">Shift : {{ session()->get('shift') }}, Line : {{ session()->get('line') }}</span>
                            </h3>
                        </div>
                        <div class="card-toolbar">
                            {{-- Buka jika butuh scan --}}
                            {{-- <button class="MulaiScan btn btn-dark"><i class="fas fa-camera"></i> Mulai Scan</button>
                            <button class="StopCamera hide btn btn-primary"><i class="fas fa-times"></i> Tutup Kamera</button> --}}
                            <a href="/prn2-lp/traceability-material/create" class="btn btn-info">Tambah Data</a>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-12 col-sm-12">
                <div class="card">
                    <div class="card-body hide cardscan text-center">
                        <video id="reader" style="width: 400px; margin-left: auto; border-radius: 10px;"></video>
                    </div>
                </div>
            </div>
            <div class="col-12 col-sm-12">
                <div class="card card-custom">
                    <div class="card-header">
                        <div class="card-title">
                            <h3 class="card-label">
                                Data Traceability Material
                            </h3>
                        </div>
                    </div>
                    <div class="card-body">
                        <div class="table-responsive">
                            <table id="tableTraceability" class="table table-striped table-bordered" style="width:100%;">
                                <thead>
                                    <tr>
                                        <th rowspan="2" class="text-center cup_sleeve">Cup Sleeve</th>
                                        <th colspan="2" class="text-center cup_sleeve">Tgl/Shift Produksi</th>
                                        <th rowspan="2" class="text-center lid_cup">Lid Cup</th>
                                        <th colspan="2" class="text-center lid_cup">No Identitas</th>
                                        <th rowspan="2" class="text-center garpu">Garpu</th>
                                        <th colspan="2" class="text-center garpu">Tgl/Shift Produksi</th>
                                        <th rowspan="2" class="text-center supplier">Supplier</th>
                                        <th rowspan="2" class="text-center supplier">Nomor Lot</th>
                                    </tr>
                                    <tr>
                                        <th class="cup_sleeve">Tanggal</th>
                                        <th class="cup_sleeve">Shift</th>
                                        <th class="lid_cup">No. SPB</th>
                                        <th class="lid_cup">No. Batch</th>
                                        <th class="garpu">Tanggal</th>
                                        <th class="garpu">Shift</th>
                                    </tr>
                                </thead>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection
```

Date: 13-09-2024