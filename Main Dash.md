 
```dataview
table without id 
    ("**" + key + "**") as "Metric",
    length(rows) as "Count"
from ""
flatten file.tasks as task
group by choice(task.completed, "✅ Completed", "🔄 Pending") as key
```
---
## 🚨 Priority Views
### ⚠️ Overdue Tasks
```dataview
table task.text as "Task",
      task.due as "Due Date",
      split(file.path, "/")[1] as "Category",
      file.link as "File"
from ""
flatten file.tasks as task
where task.due and !task.completed
where task.due < date(today)
sort task.due asc
```

### 🎯 Due Today
```dataview
table task.text as "Task",
      split(file.path, "/")[1] as "Category", 
      file.link as "File"
from ""
flatten file.tasks as task  
where task.due and !task.completed
where task.due = date(today)
```

### 📅 Due This Week
```dataview
table task.text as "Task",
      task.due as "Due Date", 
      split(file.path, "/")[1] as "Category"
from ""
flatten file.tasks as task
where task.due and !task.completed
where task.due >= date(today) and task.due <= date(today) + dur(7 days)
sort task.due asc
```

### 📅 Due Next Week
```dataview
table task.text as "Task",
      task.due as "Due Date", 
      split(file.path, "/")[1] as "Category",
      file.link as "File"
from ""
flatten file.tasks as task
where task.due and !task.completed
where task.due >= date(today) 
and task.due >= date(today) + dur(7 days)
and task.due < date(today) + dur(14 days)
sort task.due asc
```

---
## 📈 Progress Tracking

### Recently Completed (Last 7 Days)
```dataview
table task.text as "Task",
      task.completion as "Completed Date",
      split(file.path, "/")[1] as "Category",
      file.link as "File"
from ""
flatten file.tasks as task
where task.completed and task.completion >= date(today) - dur(7 days)
sort task.completion desc
```

### All Tasks Summary
```dataview
table split(file.path, "/")[1] as "Category",
      length(filter(file.tasks, (t) => t.completed)) as "✅ Done",
      length(filter(file.tasks, (t) => !t.completed)) as "⏳ Pending", 
      length(file.tasks) as "📊 Total",
      round((length(filter(file.tasks, (t) => t.completed)) / length(file.tasks)) * 100, 1) + "%" as "🎯 Progress"
from ""
where file.tasks
sort file.path asc
```
---
## 📅 Monthly Task Overview
#### Current Month Tasks
```dataview
table task.text as "Task", choice(task.completed, "✅ Done", "⏳ Pending") as "Status", task.due as "Due Date", split(file.path, "/")[1] as "Category", file.link as "File" from "" flatten file.tasks as task where task.due where date(task.due).month = date(today).month and date(task.due).year = date(today).year sort task.due asc
```
#### Next Month Tasks
```dataview
table task.text as "Task", choice(task.completed, "✅ Done", "⏳ Pending") as "Status", task.due as "Due Date", split(file.path, "/")[1] as "Category", file.link as "File" from "" flatten file.tasks as task where task.due where date(task.due).month = (date(today) + dur(1 month)).month sort task.due asc
```

---

```dataview
table task.text as "Task"
from "Work" or "Coding"
flatten file.tasks as task
where task and !task.completed and (contains(task.text, "⏫") or contains(task.text, "🔼") or contains(task.text, "🔽"))
sort contains(task.text, "⏫") desc, contains(task.text, "🔼") desc, contains(task.text, "🔽") desc
```

## 🚀 Tasks by Priority

```dataview
table task.text as "Task",
      task.due as "Due Date", 
      split(file.path, "/")[1] as "Category",
      file.link as "File",
      task.priority as "Priority"
from ""
flatten file.tasks as task
where task.priority = "high priority"
sort task.due asc
```

## 🏷️ Tasks by Tags

```dataview
table tag as "🏷️ Tag",
      length(rows) as "📊 Count",
      map(rows, (r) => r.task.text + " (" + choice(r.task.completed, "✅", "⏳") + ")") as "📋 Tasks"
from "Work" or "Coding"
flatten file.tasks as task
flatten task.tags as tag
where tag
group by tag
sort tag asc
```

---
## ✅ Task Done
### 🔝 5 Recently Completed
```dataview
task
from ""
where completed
sort file.mtime desc
limit 5
```

---
### 📜 All task
```dataview
task
from ""
where completed
sort file.mtime desc
```
