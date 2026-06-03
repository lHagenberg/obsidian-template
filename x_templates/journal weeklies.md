<%*
const title = tp.file.title;
const match = title.match(/y(\d{2})w(\d{2})/);
const year      = match[1];
const week      = match[2];
const start     = moment(`20${year}-W${week}-1`, 'GGGG-[W]WW-E').format('YYYY-MM-DD');
const end       = moment(`20${year}-W${week}-7`, 'GGGG-[W]WW-E').format('YYYY-MM-DD');
const lastStart = moment(`20${year}-W${week}-1`, 'GGGG-[W]WW-E').subtract(1, 'weeks').format('YYYY-MM-DD');
-%>
---
type: journal/weekly
---
# to do this week
```tasks
scheduled on or after <% start %>
scheduled on or before <% end %>
group by function task.scheduled.format("dddd YYYY-MM-DD")
path includes 1.dailies
```
# back log
```tasks
happens on or after <% lastStart %>
filter by function return task.scheduled.moment?.isBefore(moment(), 'day') && task.scheduled.moment?.isSameOrBefore(moment('<% end %>'), 'day') || false 
not done
path includes 1.dailies
```
# macro tasks
- [ ]