#Tech 
# Table of Content
- [[#Introduction|Introduction]]
- [[#Studi Kasus|Studi Kasus]]
	- [[#Studi Kasus#Table|Table]]
	- [[#Studi Kasus#Datatable|Datatable]]
	- [[#Studi Kasus#Tampilkan Tombol Submit|Tampilkan Tombol Submit]]
	- [[#Studi Kasus#Logic JS nya|Logic JS nya]]
	- [[#Studi Kasus#Logic Backend|Logic Backend]]
- [[#Update Terbaru|Update Terbaru]]

## Introduction
Ada kebutuhan ketika kita ceklis data yg terceklis akan terkirim ke Backend.

## Studi Kasus
![[Screenshot from 2024-05-30 08-05-43.png]]
Studi kasus kita ambil saya akan melakukan ceklis untuk karyawan yg akan keluar.

### Table
```html
<table id="tableAjax" class="table table-bordered" style="width:100%">
	<thead>
		<tr>
			<th data-ordering="false" style="width: 20%;">NIK</th>
			<th data-ordering="false" style="width: 30%;">Nama</th>
			<th data-ordering="false" style="width: 5%;">Pilih</th>
			<th data-ordering="false" style="width: 5%;">Loker</th>
			<th data-ordering="false" style="width: 10%;">ID Kartu</th>
			<th data-ordering="false">Dept</th>
			<th data-ordering="false">Alasan Keluar</th>
			<th data-ordering="false">Tanggal Keluar</th>
		</tr>
	</thead>
</table>

<button type="button" class="btn btn-secondary custom-toggle active mb-3" data-bs-toggle="button" id="btnSubmit">
	<span class="icon-on"><i class="ri-alarm-line align-bottom me-1"></i> Mohon Tunggu</span>
	<span class="icon-off">Submit</span>
</button>
```

Kita simple nya buat table tanpa isi nya (tbody nya), dan tombol submit.

### Datatable
```javascript
let table = $("#tableAjax").dataTable({
	processing: false,
	serverSide: true,
	paging: false,
	ajax: {
		type: "GET",
		url: "/hr-connect/dept-ga/karyawan-keluar/getData"
	},
	columns: [
		{ data: 'nik' },
		{ data: 'nama' },
		{
			render: function(data, type, row){
				return `
				<center>
					<input type="checkbox" class="checklist" data-nik="${row.nik}" value="${row.id}">
				</center>
				`;
			}
		},
		{
			render: function(data, type, row){
				let status = '';

				if(row.out_complete == 'Y'){
					status = 'checked disabled';
				}else{
					status = 'disabled';
				}

				return `
				<center>
					<input type="checkbox" class="check_id_loker" ${status}>
				</center>
				`;
			}
		},
		{
			render: function(data, type, row){
				let status = '';

				if(row.out_complete == 'Y'){
					status = 'checked disabled';
				}else{
					status = 'disabled';
				}

				return `
				<center>
					<input type="checkbox" class="check_id_card" ${status}>
				</center>
				`;
			}
		},
		{ data: 'kode_divisi' },
		{ data: 'alasan_keluar' },
		{ data: 'tanggal_keluar' },
	]
});
```

### Tampilkan Tombol Submit
Kita buat minimal 1 ceklis untuk menampilkan tombol submit.

```javascript
$(document).on("change",".checklist", function(){
	$("#btnSubmit").show();
});
```

### Logic JS nya
```javascript
$(document).on("click","#btnSubmit", function(){
	let dataToSend = [];

	$("#tableAjax tbody input[type=checkbox].checklist:checked").each(function(){
		let row = $(this).closest('tr');

		let checklistId = $(this).val();
		let status = 'check';

		dataToSend.push({checklistId: checklistId, status: status});
	});

	$.ajax({
		type: "POST",
		url: "/hr-connect/dept-ga/karyawan-keluar/update",
		data: {
			data: dataToSend
		},
		success: function(response){
			Toastify({
				text: "Berhasil memperbarui data karyawan keluar!",
				duration: 3000,
				gravity: "top",
				position: 'right',
				backgroundColor: "linear-gradient(to right, #28a745, #218838)",
			}).showToast();

			table.api().draw();

			$("#btnSubmit").hide();
		},
		error: function(xhr, status, error){
			console.error(xhr.responseText);
		}
	})
});
```

Penjelasan :
1. Buat variable array kosong `let dataToSend = [];`
2. looping checklist nya, ambil value nya, pakai variable `dataToSend.push`
3. next nya pakai ajax buat send data.

### Logic Backend
```php
public function update(Request $req)
{
	$data = $req->input('data');

	if(!empty($data)){
		foreach ($data as $item) {
			if($item['checklistId'] !== 'on'){
				HrKaryawan::where('id', $item['checklistId'])->update([
					'out_complete' => $item['status'] =='check' ? 'Y' : 'N'
				]);
			}
		}

		return response()->json(['success' => true, 'message' => 'Data berhasil dikirim.']);
	}else {
		return response()->json(['success' => false, 'message' => 'Tidak ada data yang dikirim.']);
	}
}
```

Untuk menerima data yg di kirim dari Ajax `data: dataToSend` ini akan diterima Laravel dalam bentuk request `$req->data`. Nama request disesuaikan. jika di ajax misal `data:{ nama: nama, umur: umur, hobi: hobi }` maka di laravel pakai `$req->nama`.

## Update Terbaru
Contoh memilih data untuk dihapus... Update:
- membungkus notifikasi dengan function agar tidak ada kode yg di ulang-ulang.

```javascript
$("#btnRemoveChoose").hide();

let table = $("#masterBankTable").DataTable({
    searching: false,
    serverSide: true,
    info: false,
    paging: false,
    ordering: false,
    ajax: {
        type: "GET",
        url: "/su/master/bank/getData"
    },
    columns: [
        {
            render: function(data, type, row){
                return `<input type="checkbox" class="form-check-input checkwish" value="${row.id}">`;
            }
        },
        {
            data: "picture",
            render: function(data,type, row){
                return `<img src="/assets/images/banks/${data}" class="img-fluid rounded" width="50">`
            }
        },
        {
            data: "nama_bank"
        },
        {
            data: "created_at",
            render: function(data, type, row){
                let created_at = moment(data).format('LL');
                return `${created_at}`;
            }
        },
        {
            data: "updated_at",
            render: function(data, type, row){
                let updated_at = moment(data).format('LL');
                return `${updated_at}`;
            }
        },
        {
            render: function(data, type, row){
                return `<button class="btn btn-sm btn-danger btnRemove" data-id="${row.id}"><i class="mdi mdi-trash-can"></i></button>`;
            }
        },
    ]
});

$(document).on("change", ".checkwish", function() {
    let anyChecked = $('.checkwish:checked').length > 0;
    if (anyChecked) {
        $('#btnRemoveChoose').show();
    } else {
        $('#btnRemoveChoose').hide();
    }
});

function notifySuccess(message){
    Swal.fire({
        title: "Berhasil",
        icon: "success",
        text: message
    });

    $("input").val('');
    $(".modalHush").modal("hide");
    table.ajax.reload();
}

function notifyError(message){
    Swal.fire({
        title: "Error",
        icon: "error",
        html: message
    });
}

$(document).on("click", "#btnRemoveChoose", function(){
    let dataToSend = [];
    $("#masterBankTable tbody input[type=checkbox].checkwish:checked").each(function(){
        checkwishId = $(this).val();

        dataToSend.push({checkwishId});
    });

    Swal.fire({
        title: "Konfirmasi",
        icon: "info",
        text: "Yakin ingin menghapus data?",

        showCancelButton: true,
        confirmButtonText: "Hapus",
        confirmButtonColor: "#ED5E5E"
    }).then((result) => {
        if(result.isConfirmed){
            $.ajax({
                type: "DELETE",
                url: "/su/master/bank/removeChoose",
                data: {
                    data: dataToSend
                },
                success: function(res){
                    notifySuccess(res.message);
                }
            });
        }
    });
});
```

Logic PHPnya

```php
public function removeChoose(Request $req)
{
	$data = $req->input('data');

	if(!empty($data)){
		foreach ($data as $item) {
			if($item['checkwishId'] !== 'on'){
				MasterBank::where('id', $item['checkwishId'])->delete();
			}
		}

		return response()->json(['success' => true, 'message' => 'Data berhasil dikirim.']);
	}else {
		return response()->json(['success' => false, 'message' => 'Tidak ada data yang dikirim.']);
	}
}
```

Date : 30-05-2024