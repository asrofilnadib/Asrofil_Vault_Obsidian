#Tech 
## Introduction
Intinya gue ingin tinggi select2 harus sejajar. Biar enak diliat aja.

![[Pasted image 20250409103348.png]]

Jadi seperti ini:

![[Pasted image 20250409103250.png]]

## Kode nya
```css
.select2-container--default .select2-selection--single {
    height: calc(2.25rem + 2px);
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
}

.select2-container--default .select2-selection--single .select2 selection__rendered {
    line-height: 2.25rem;
}

.select2-container--default .select2-selection--single .select2-selection__arrow {
    height: 2.25rem;
}
```

## Full nya

```html
@extends('pas-downtime.admin.layout')

@section('title', 'Dashboard')

@push('styles')
<link rel="stylesheet" href="/assets/velzon/libs/apexcharts/apexcharts.css">
<link rel="stylesheet" href="{{ url('/assets/plugins/custom/datatables/datatables.bundle.css') }}">
<link rel="stylesheet" href="{{ asset('assets/css/select2.min.css') }}">
<style>
.hide{
    display: none !important;
}

.select2-container--default .select2-selection--single {
    height: calc(2.25rem + 2px); 
    border: 1px solid #ced4da; 
    border-radius: 0.25rem; 
}

.select2-container--default .select2-selection--single .select2-selection__rendered {
    line-height: 2.25rem; 
}

.select2-container--default .select2-selection--single .select2-selection__arrow {
    height: 2.25rem; 
}
</style>
@endpush

@section('content')
<div class="container-fluid">
    <div class="card">
        <div class="card-body">
            <h2 class="mb-0">Report Monitoring</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-12">
            <div class="card">
                <div class="card-body">
                    <div class="row align-items-center mb-5">
                        <div class="col-md-2">
                            <label for="start">Dari:</label>
                            <input type="date" id="start" class="form-control">
                        </div>
                    
                        <div class="col-md-2">
                            <label for="end">Sampai:</label>
                            <input type="date" id="end" class="form-control">
                        </div>

                        <div class="col-md-2">
                            <label for="mesin">Mesin</label>
                            <select id="mesin" class="form-select">
                                <option value="">Semua</option>
                                @foreach($mesin as $ms)
                                <option value="{{ $ms->machine_code }}">{{ $ms->machine_name }}</option>
                                @endforeach
                            </select>
                        </div>
                       
                        <div class="col-md-2">
                            <label for="kerusakan">Kerusakan</label>
                            <select id="kerusakan" class="form-select">
                                <option value="">Semua</option>
                                @foreach($trouble as $tr)
                                <option value="{{ $tr->trouble_code }}">{{ $tr->trouble_name }}</option>
                                @endforeach
                            </select>
                        </div>
                    
                        <div class="col-md-2">
                            <button class="btn btn-primary mt-4" onClick="renderTable()">Cari</button>&nbsp;
                            <button class="btn btn-danger mt-4" onClick="resetFilters()">Show All</button>
                        </div>
                    </div>
                    <table class="table" id="tableMonitoring">   
                        <thead>
                            <tr>
                                <th>Tipe</th>
                                <th>Mesin</th>
                                <th>Equipment</th>
                                <th>Kerusakan</th>
                                <th>CreatedAt</th>
                                <th>Teknisi</th>
                                <th>Running Time</th>
                            </tr>
                        </thead>
                    </table>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection

@push('scripts')
<script src="/assets/velzon/libs/apexcharts/apexcharts.min.js"></script>
<script src="/assets/velzon/libs/moment/moment.js"></script>
<script src="{{ url('/') }}/assets/js/scripts.bundle.js?v=7.0.5"></script>
<script src="{{ url('/assets/plugins/custom/datatables/datatables.bundle.js') }}"></script>
<script src="{{ asset('assets/plugins/global/select2.full.min.js') }}"></script>
<script>
$("#mesin").select2();
$("#kerusakan").select2();

let startDate = $('#start').val();
let endDate = $('#end').val();
let mesin = $('#mesin').val();
let kerusakan = $('#kerusakan').val();

function renderTable() {
    startDate = $('#start').val();
    endDate = $('#end').val();
    mesin = $('#mesin').val();
    kerusakan = $('#kerusakan').val();

    tableMonitoring(startDate, endDate, mesin, kerusakan);
}

function resetFilters() {
    $('#start').val('');
    $('#end').val('');
    $('#mesin').val('').trigger('change');
    $('#kerusakan').val('').trigger('change');

    tableMonitoring('', '', '', '');
}

function tableMonitoring(startDate, endDate, mesin, kerusakan) {
    $("#tableMonitoring").DataTable({
        bDestroy: true,
        ordering: false,
        dom: "<'row'<'col-sm-6 text-left'B><'col-sm-6 text-right'f>>" +
             "<'row'<'col-sm-12'tr>>" + 
             "<'row'<'col-sm-12 col-md-5'i><'col-sm-12 col-md-7'p>>",
        buttons: [
            {
                extend: 'excel',
                text: '<i class="mdi mdi-file-excel"></i> Export to Excel', 
                className: 'btn btn-success',
                messageTop: 'Dashboard Downtime System',
                filename: 'Dashboard Downtime System'
            },
        ],
        ajax: {
            type: "GET",
            url: "/pas-downtime/admin/report/getDataMonitoringDetail",
            data: {
                start: startDate,
                end: endDate,
                mesin: mesin,
                kerusakan: kerusakan
            },
        },
        columns: [
            { data: "type" },
            { data: "machine.machine_name" },
            { data: "equipment.equipment_name" },
            { data: "trouble.trouble_name" },
            { 
                data: null,
                render: function(data, type, row){
                    return moment(row.created_at).format("DD MMM YYYY");
                }
            },
            { data: "creator_name" },
            { 
                data: null,
                render: function(data, type, row){
                    var createdAt = moment(row.created_at); 
                    var inProgressAt = row.in_progress_at ? moment(row.in_progress_at) : null; 
                    var now = moment(); 

                    var runningTime = "-"; 
                    if (inProgressAt) {
                        var elapsedSeconds = inProgressAt.diff(createdAt, 'seconds'); 
                        var minutes = Math.floor(elapsedSeconds / 60); 
                        var seconds = elapsedSeconds % 60; 
                        runningTime = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`; 
                    } else {
                        var elapsedSeconds = now.diff(createdAt, 'seconds');
                        var minutes = Math.floor(elapsedSeconds / 60);
                        var seconds = elapsedSeconds % 60;
                        runningTime = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`; 
                    }

                    return runningTime;
                }
            },
        ]
    });
}tableMonitoring(startDate, endDate, mesin, kerusakan);
</script>
@endpush
```