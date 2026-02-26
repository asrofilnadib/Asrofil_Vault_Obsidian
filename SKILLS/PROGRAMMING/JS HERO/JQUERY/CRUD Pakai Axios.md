#Tech 
# Table of Content
- [[#Axios|Axios]]
- [[#Read Data|Read Data]]
- [[#Create Data|Create Data]]
- [[#Update|Update]]
- [[#Remove Data|Remove Data]]
- [[#Controller Fullnya|Controller Fullnya]]

## Axios
Axios adalah sebuah library JavaScript yang digunakan untuk melakukan **HTTP request** dari browser atau Node.js. Library ini menyediakan cara yang sederhana untuk mengirim permintaan HTTP ke server, mengelola respons, dan mengelola kesalahan secara efisien. Axios sangat populer dalam pengembangan aplikasi web modern karena memiliki fitur-fitur yang kuat dan mudah digunakan.

Berikut beberapa fitur utama dari Axios:
1. **Mendukung Berbagai Browser**: Axios dapat digunakan baik di browser maupun di lingkungan Node.js.
2. **Mendukung Promise**: Axios menggunakan Promise API modern, yang memungkinkan Anda untuk menangani respons asinkron dengan lebih mudah dan terstruktur menggunakan `then` dan `catch`.
3. **Melakukan Request HTTP**: Axios mendukung semua metode HTTP utama seperti GET, POST, PUT, DELETE, dll. Anda dapat dengan mudah menentukan jenis request yang ingin Anda kirim.
4. **Transformasi Data**: Anda dapat melakukan transformasi data request dan respons menggunakan interceptor, misalnya untuk mengubah format data atau menambahkan header secara dinamis.
5. **Penanganan Kesalahan**: Axios menyediakan cara untuk menangani berbagai jenis kesalahan yang mungkin terjadi selama proses pengiriman request atau penerimaan respons dari server.
6. **Intersep Permintaan dan Respons**: Anda dapat menambahkan interceptor untuk mengelola permintaan dan respons secara global sebelum mereka dikirim atau diterima.
7. **Mudah Digunakan**: Axios dirancang agar mudah digunakan dengan sintaks yang intuitif dan dokumentasi yang baik, membuatnya sangat populer di komunitas pengembang.

Contoh penggunaan Axios seperti yang telah kita diskusikan sebelumnya adalah untuk mengirimkan permintaan HTTP dari frontend JavaScript (misalnya dari Vue.js, React, atau halaman web statis) ke backend server (misalnya menggunakan Laravel, Express.js, atau Flask).

Secara keseluruhan, Axios adalah pilihan yang populer dan andal untuk melakukan komunikasi HTTP di aplikasi web modern, dengan fokus pada kejelasan API dan kemudahan integrasi dengan berbagai teknologi frontend dan backend.

## Read Data
Menggunakan data table

```javascript
let table = $("#tableMesin").DataTable({
    info: false,
    paging: false,
    ordering: false,
    ajax: {
        type: "GET",
        url: "/fl_produksi/master/mesin/getData"
    },
    columns: [
        {
            data: 'no_mesin'
        },
        {
            width: '10%',
            render: function(data, type, row){
                return `
                <center>
                    <button class="btn btn-sm btn-success btnEditMesinModal" data-id="${row.id}" data-mesin="${row.no_mesin}"><i class="mdi mdi-pencil"></i></button>
                    <button class="btn btn-sm btn-danger btnRemoveMesin" data-id="${row.id}"><i class="mdi mdi-trash-can"></i></button>
                </center>
                `;
            }
        }
    ]
});
```

## Create Data
Kurang lebih akan seperti ini

```javascript
$(document).on("click", "#btnSubmitMesin", function(){
    let noMesinForm = $("#noMesinForm").val();

    axios.post("/fl_produksi/master/mesin/store", {
        no_mesin: noMesinForm,
    }).then(res => {
        $("#modal").modal("hide");

        Swal.fire({
            title: "Berhasil",
            icon: "success",
            text: res.message
        });

        table.ajax.reload();
    }).catch(error => {
        alert(error);
    });
});
```

## Update
Logicnya sama dengan Create data,

```javascript
$(document).on("click", ".btnEditMesinModal", function(){
    let id = $(this).data("id");
    let mesin = $(this).data("mesin");

    $("#judulModal").text("Edit Data Master");

    $("#contentModal").empty();

    $("#contentModal").append(`
        <div class="mt-1">
            <div class="row">
                <div class="col-lg-12">
                    <label for="">Nomor Mesin</label><br>
                    <input type="number" class="form-control" id="noMesinForm" value="${mesin}">
                </div>
                <div class="col-lg-12">
                    <button class="btn btn-success rounded-pill mt-3" id="btnEditMesin">
                        Update
                    </button>
                </div>
            </div>
        </div>
    `);

    $(document).on("click", "#btnEditMesin", function(){
        let mesinForm = $("#noMesinForm").val();

        axios.put("/fl_produksi/master/mesin/update", {
            id: id,
            no_mesin: mesinForm
        }).then(res => {
            $("#modal").modal("hide");

            Swal.fire({
                title: "Berhasil",
                icon: "success",
                text: res.message
            });

            table.ajax.reload();
        }).catch(error => {
            alert(error);
        });
    });

    $("#modal").modal("show");
});
```

## Remove Data
Disini agak sedikit berbeda karena diharuskan menggunakan property data.

```javascript
$(document).on("click", ".btnRemoveMesin", function(){
    let id = $(this).data("id");

    Swal.fire({
        title: "Konfirmasi",
        icon: "question",
        text: "Yakin ingin menghapus data?",
        showDenyButton: true,
        confirmButtonText: "Hapus",
        denyButtonText: "Batalkan"
    }).then((result) => {
        if(result.isConfirmed){
            axios.delete("/fl_produksi/master/mesin/remove", {
                data: {
                    id: id
                }
            }).then(function(res){
                Swal.fire({
                    title: "Berhasil",
                    icon: "success",
                    text: res.message
                });

                table.ajax.reload();
            }).catch(function(error){
                alert(error);
            });
        }else if(result.isDenied){
            Swal.fire({
                title: "Cancel",
                icon: "info",
                text: "Hapus data dibatalkan!"
            })
        }
    });
});
```

## Controller Fullnya
```php
public function getData()
{
	$fl_produksi = FlProduksiMasterMesin::all();

	return Datatables::of($fl_produksi)->make(true);
}

public function index()
{
	return view('fl_produksi.master.mesin.list');
}

public function store(Request $req)
{
	$validator = Validator::make($req->all(), [
		'no_mesin' => 'required'
	],[
		'no_mesin.required' => 'Nomor Mesin harus diisi!'
	]);

	if($validator->fails()){
		return response()->json([
			'errors' => $validator->errors()
		], 422);
	}else{
		FlProduksiMasterMesin::updateOrCreate([
			'no_mesin' => $req->no_mesin,
		],[
			'no_mesin' => $req->no_mesin,
		]);

		return response()->json([
			'message' => 'Berhasil menambahkan data!'
		]);
	}
}

public function update(Request $req)
{
	$validator = Validator::make($req->all(), [
		'no_mesin' => 'required'
	],[
		'no_mesin.required' => 'Nomor Mesin harus diisi!'
	]);

	if($validator->fails()){
		return response()->json([
			'errors' => $validator->errors()
		], 422);
	}else{
		FlProduksiMasterMesin::where('id', $req->id)->update([
			'no_mesin' => $req->no_mesin,
		]);

		return response()->json([
			'message' => 'Berhasil memperbarui data!'
		]);
	}
}

public function destroy(Request $req)
{
	$id = $req->id;

	FlProduksiMasterMesin::where('id', $id)->delete();

	return response()->json([
		'message' => 'Berhasil menghapus data'
	]);
}
```

Date : 26-06-2024