<%* 
const title = tp.file.title; 
const match = title.match(/y(\d{2})w(\d{2})d(\d)/); 
const year = match[1]; 
const week = match[2]; 
const day = match[3];
const today = moment(`20${year}-W${week}-${day}`, 'GGGG-[W]WW-E').format('YYYY-MM-DD');
const yesterday = moment(`20${year}-W${week}-${day}`, 'GGGG-[W]WW-E').subtract(1, 'days').format('YYYY-MM-DD');
const thisWeek = `y${year}w${week}`; 
-%>---
type: journal/daily
---
- [ ] check [[<% thisWeek %>]] backlog 🏁 delete 
- [ ] plan tomorrow 🏁 delete
- [ ] check agenda for tomorrow 🏁 delete
# planned today
- [ ]
# from yesterday
```tasks
happens on <% yesterday %>
filter by function ! task.isDone
```
# notes
