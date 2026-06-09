<%* 
const title = tp.file.title; 
const match = title.match(/y(\d{2})m(\d{2})-(\w{3})/); const year = match[1]; 
const month = match[2]; 
const start = moment(`20${year}-${month}-01`, 'YYYY-MM-DD').format('YYYY-MM-DD'); 
const end = moment(`20${year}-${month}-01`, 'YYYY-MM-DD').endOf('month').format('YYYY-MM-DD'); 
-%>---
type: journal/weekly
---
# this month
```tasks
scheduled on or after <% start %> 
scheduled before <% end %>
path includes 2.weeklies
sort by function task.file.filenameWithoutExtension
```

# macro tasks
- [ ]  
