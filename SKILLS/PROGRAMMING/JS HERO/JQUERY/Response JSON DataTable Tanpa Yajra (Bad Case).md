#Tech 
# Table of Content
- [[#Introduction|Introduction]]

## Introduction
Kali ini saya ingin mencoba tanpa yajra ternyata bisa. Jadi intinya di backend itu kita return json nya seperti ini.

```php
public function getData()
{
	$master_bank = MasterBank::all();
	return response()->json([
		'data' => $master_bank
	]);
}
```

Di viewsnya akan seperti ini...

```javascript
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
                return `<input type="checkbox" class="form-check-input" value="${row.id}">`;
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
```

Date : 04-07-2024