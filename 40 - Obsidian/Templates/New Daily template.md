---
creation date: <% tp.file.creation_date() %>
modification date: <%+ tp.file.last_modified_date("dddd Do MMMM YYYY HH:mm:ss") %>
tags:
  - DailyNote
banner: "40 - Obsidian/Attachments/banners/daily-note-banner.gif"
cssclasses:
  - noyaml
banner_icon: 📝
banner_x: 0.5
banner_y: 0.38
week: "<% tp.date.now('YYYY-[W]WW') %>"
Summary: ""
---

# <% tp.file.title %>

<< [[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>]] | [[<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>]]>>

## Overview
- Summary :: 

## Tasks

#### Overdue
```tasks
not done
due before <% tp.date.now("YYYY-MM-DD") %>
```

#### Due Today
```tasks
not done
due on <% tp.date.now("YYYY-MM-DD") %>
```

#### Due This Week
```tasks
not done
due after <% tp.date.now("YYYY-MM-DD") %>
due before <% tp.date.now("YYYY-MM-DD", 7) %>
```

## Agenda
