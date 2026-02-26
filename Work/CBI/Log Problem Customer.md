# 📝 Log Problem - Bapak Syafiq

> Modul pengelolaan problem log dari customer, dilengkapi endpoint API dan visualisasi chart

## 📌 Table Adjustment
- `pic_cbi` ➜ diubah menjadi **nama pelapor** #Easy 
- `tanggal_komplain` ➜ diubah menjadi **tanggal problem** #Easy 
- Fields `pic_customer`, `pic_user`, dan `sumber info` **dihilangkan** #Easy 
- Action buttons untuk: #Struggle 
  - Modal `PICA`
  - Status: `Approval / Rejected`, `Open / Closed` 

## 🧾 Form Adjustment
- Signature dihapus #Easy 
- Charger & Battery dapat ditambah dinamis #Struggle 

## 🌐 API Endpoints #API 
- GET all logs:  
  `{{base_url}}/api/v1/form/data-log`
- GET log by ID:  
  `{{base_url}}/api/v1/form/data-log/1`
- GET column chart data (all):  
  `{{base_url}}/api/v1/chart/column`
- GET column chart by specific problem:  
  `{{base_url}}/api/v1/chart/column/{{jenis_product}}/{{spesific_problem}}`
- GET pie chart data:  
  `{{base_url}}/api/v1/chart/pie`

## 📊 Visualizations #Dashboard 
- Pie Chart & Column Chart untuk monitoring #Struggle 
- Basis data dari API endpoint log problem #Easy 

---

## 🏷️ Tags
#Struggle #Hard #API #LogProblem
