# 📄 Portal Lab QC - Department

### 🌐 [Portal Lab QC](https://portal3.incoe.astra.co.id/lab-qc/public/login)

---


- [x] add year filtering in data dashboard ✅ 2025-02-13
- [x] fixed no_master_log in lab/test/update/(:num) ✅ 2025-02-13
- [x] add year filter button on column chart in dashboard and data-battery chart ✅ 2025-03-18
- [x] add column chart testing battery by year ✅ 2025-03-24
- [x] in form page, add field need_test, keperluan, customer, logo_kan to testing form ✅ 2025-06-26
- [x] auto-generate testing request fields for marketing selection ✅ 2025-06-26
- [x] create button on kadep Approve form for navigate to testing report app ✅ 2025-06-27
- [x] move need_test field to each sample entry in testing form ✅ 2025-07-02
- [x] allow selecting existing or adding new customer ✅ 2025-07-03
- [x] fixed auto-generate logic field need a testing in form ✅ 2025-07-14


- [x] debug analysis when form created it's not showing in lab test data battery ✅ 2025-07-28
	- ketika form dibuat, pada saat penyimpanan data battery tidak dimuat no_master_log. Sedang query model untuk menampilkan data battery hanya menampilkan no_master_log yang not null 
```php
for ($i = 0, $iMax = count($data['dataSample']); $i < $iMax; $i++) { // looping to get data from dataSample  
  $jumlahBattery = $data['dataSample'][$i]['jumlahBattery']; // get the jumlahBattery  
  for ($b = 0; $b < $jumlahBattery; $b++) { // loop based on jumlahBattery  
   $jenis = implode(', ', $data['dataSample'][$i]['jenisPengujian']); // convert array to string  
   $dataBattery = [  
    'id_form' => $this->formModel->getInsertID(), // getInsertID() to get the id that was just inserted  
    'battery_name' => $data['dataSample'][$i]['namaBattery'],  
    'plate_type' => $data['dataSample'][$i]['tipePlate'],  
    'plate_qty' => $data['dataSample'][$i]['jumlahPlate'],  
    'separator' => $data['dataSample'][$i]['separator'],  
    'testing_name' => $jenis,  
    'testing_status' => 'Belum Diterima',  
    'created_at' => date('Y-m-d H:i:s'),  
    'updated_at' => date('Y-m-d H:i:s')  
   ];  
   $this->batteryModel->insert($dataBattery); // insert data into database  
  }  
}
```