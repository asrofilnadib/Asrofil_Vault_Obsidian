# 🧪 Testing Report - Bapak Abub
> Summary of project flow and revisions related to Testing Report portal.  
> Project Location: `QA/Lab/Testing Report`

### 🌐 [Portal Testing Report](https://portal3.incoe.astra.co.id/qa-testing-report/public/testing)

## 📌 Project Overview
- Requestor fetches department via API (select2)
- Customer input is free-text
- KAN Logo: boolean (Yes/No)
- Battery Type: multiple entries allowed
- Existing Report: dropdown with 2 options: `Available`, `Unavailable, Need Sample`
- Actual Dates: request, checking data, sample received, testing finished, report, submit
- Status & flow based on availability of fields
- Action: full edit form

---

## 🧭 Flow Notes

### Existing Report Flow
- `Available`: sample & testing => N/A; report & submit => "In Progress"
- `Unavailable`: sample > testing > report > submit (ordered flow)
- If `checking`, `sample`, `testing` are empty: respective waiting statuses
- Status closes when:
  - `Available`: only `report` & `submit` needed
  - `Unavailable`: all fields must be filled
- #Struggle 

---

## 📅 #Meeting

### 🗓️ Meeting 19-06-2025 #AddFeatures #Revision 
- Implement conditional disabling of fields based on `Existing Report`
- Improve flow status logic  
  #Easy 

### 🗓️ Meeting 26-06-2025 #AddFeatures
- On Lab-QC project, if dept = "Marketing" → show 4 questions
- If answer to Q1 is "Need test report" → auto-generate data in Testing Report
- Add redirection button between Lab-QC ↔ Testing Report  
  #Struggle

### 🗓️ Meeting 02-07-2025 #AddFeatures  #Revision
**Lab-QC:**
- Each sample entry duplicates Q1–Q4
- Make Q1 required

**Testing Report:**
- Clear date should update status correctly
- Rename page title → “Monitoring of Testing Report”
- Modal “For Customer” uses `selectize` fetching customer name  
  #Easy

**Dashboard:** #Dashboard 
- Gantt-like chart for timeline monitoring (anchor: created_at)
- External Test Report:
  - By month (issued/closed logic)
  - By customer (data grouped and counted)
  #Hard

### 🗓️ Meeting 14-07-2025 #Revision 
1. On Lab-QC, if multiple customers → generate multiple Testing data; else one #Easy 
2. Add conditional status logic per field:
	1. if _Checking_ was empty: `Waiting Checking Existing Report (PIC : Lab)`
	2. **_Existing Report_**: #Struggle 
		1. **Available**, it is doesn't need sample & testing, just take it from previous testing in database
			1. Report: `Waiting Report Testing (PIC : Lab)`
			2. Submit: `Waitin g Submit Testing Report (PIC : Lab)`
		2. **Unavailable, Need Sample**
			1. Sample: `Waiting Sample Testing (PIC : Requestor)`
			2. Testing: `Waiting Testing (PIC : Lab)`
			3. Report: `Waiting Report Testing (PIC : Lab)`
			4. Submit: `Waiting Submit Testing Report (PIC : Lab)`
	3. Status becomes **Closed** when its all fields has been filled
3. If Lab form created, auto-fill `request` field in Testing with form creation date  
  #Hard

### 🗓️ Meeting 18-07-2025 #AddFeatures 
- [x] Add `No Order` column on testing auto-generate from lab form #Easy ✅ 2025-07-18
- [x] Change requestor format to: `Departemen - PIC Requestor` `(MARKETING - Lukman)` #Easy ✅ 2025-07-18
- [x] Email notifications on each status changes #Hard ✅ 2025-07-22

##  🧠Ideas
- [x] ➕ 2025-07-24 add filtering button years inside bar chart External Testing Report by Monthly #Easy 🔼 ✅ 2025-11-05
- [x] ➕ 2025-07-28 gave notification when labqc form was created #Easy ⏫ ✅ 2025-11-05

🗓️ Meeting 04-08-2025 #Revision 
- [x] ➕ 2025-08-04 update subject email #Easy 🔽 ✅ 2025-11-05
---

## 🏷️ Tags Glossary
- `#Easy` – Small fixes, minor UI/UX, simple logic
- `#Struggle` – Moderate complexity, conditional logic
- `#Hard` – High complexity, involving cross-project logic or async/notifications

---

## 🔗 Related Notes
- [[Portal Lab-QC Logic]]
- [[Dashboard Design - Testing]]
- [[Email Notification Handler]]
- [[Auto Generation - Testing from Lab]]
