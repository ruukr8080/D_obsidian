
---
<%*
const title = tp.file.title;
const today = moment(title).format("YYYY-MM-DD");
const yesterday = moment(title).subtract(1, 'days').format("YYYY-MM-DD");
const tomorrow = moment(title).add(1, 'days').format("YYYY-MM-DD");
-%>
created: <% tp.file.creation_date() %>
> updated: <% tp.file.creation_date() %>
tag: ['DailyNote']
---

# <% today %>

<< [[<% yesterday %>]] | [[<% tomorrow %>]]>>

---
### 📅 Daily Questions

##### 🌜 Last night, after work, I...

- <% tp.file.cursor(0) %>

##### 🙌 One thing I've excited about right now is...

- 

##### 🚀 One+ thing I plan to accomplish today is...

- [ ] 

##### 👎 One thing I'm struggling with today is...

- 

---

# 📝 Notes

- 

---

### Notes created today

```dataview
List FROM "" WHERE file.cday = date("<%today%>") SORT file.ctime asc
```

### Notes modified today

```dataview
List FROM "" WHERE file.mday = date("<%today%>") SORT file.mtime asc
```
