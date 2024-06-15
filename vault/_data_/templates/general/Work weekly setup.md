<%*
const moment = tp.user.moment;
const title = await tp.system.prompt("Gib das Datum für die Woche im Format YYYY-WW ein:");
const filePath = tp.file.path(true);
let fileObject = this.app.vault.getAbstractFileByPath(filePath);
_%>
<% "---" %>
date_created: <% tp.file.creation_date() %>
date_modified: <% tp.file.last_modified_date() %>
document_type: work
tags: 
- " #work  " 
<% "---" %>
```dataviewjs
/*
    Previous/next note by week for Weekly Notes
    Adjusted to work with weekly notes in the format `YYYY-WW` using Moment.js.
*/
var none = '(none)';
var currentWeek = dv.current().file.name ? dv.current().file.name : moment().format('YYYY-WW');
var format = 'YYYY-WW'; // Define the week format
var current = '(' + moment(currentWeek, format).format(format) + ')';
var nav = [];

// Calculate previous and next weeks
var prevWeek = moment(currentWeek, format).subtract(1, 'weeks').format(format);
var nextWeek = moment(currentWeek, format).add(1, 'weeks').format(format);

// Check if files for previous and next weeks exist
var prevFile = dv.page(prevWeek);
var nextFile = dv.page(nextWeek);

nav.push(prevFile ? '[[' + prevWeek + ']]' : none);
nav.push(current);
nav.push(nextFile ? '[[' + nextWeek + ']]' : none);

dv.paragraph(nav[0] + ' ← ' + nav[1] + ' → ' + nav[2]);

```

[[Work journal Dashboard|Work Journal]] / **<% `[[${filePath.slice(0,-3)}|${fileObject.basename}]]` %>**
<% `# ${fileObject.basename}` %>

> [!important] Significant Event
> Event:: 

> [!todo]
> ```dataviewjs
> let tasks = dv.current().file.tasks
> let completedTasks = dv.current().file.tasks.where(t => t.completed)
> let incompletedTasks = dv.current().file.tasks.where(t => !t.completed)
> let percentCompleted = (completedTasks.length/tasks.length)*100
> 
> if (tasks.length > 0) {
>   dv.el("div", `
>  <progress value="${(completedTasks.length/tasks.length)*100}" max="100"></progress>
>  <small>${Math.round((completedTasks.length/tasks.length)*100, 0)}% completed (${incompletedTasks.length} tasks remaining)</small>
>  `)
> }
> ```
> ```tasks
> created <% tp.file.creation_date("YYYY-[W]ww") %>
> ```

```meta-bind-button
label: Create Task
hidden: false
class: ""
tooltip: Creates a Task
id: ""
style: default
actions:
  - type: command
    command: obsidian-tasks-plugin:edit-task

```

>[!Notes] 

<% tp.file.cursor(1) %>

<%*
const fileName = tp.file.title; // Dateiname im Format YYYY-WW
const year = parseInt(fileName.substring(0, 4)); // Jahr extrahieren
const week = parseInt(fileName.substring(5, 7)); // Woche extrahieren

function getWeekdayDates(year, week) {
    const firstDayOfYear = new Date(year, 0, 1);
    const firstThursday = new Date(firstDayOfYear.getTime() + (3 - (firstDayOfYear.getDay() + 6) % 7) * 86400000);
    const january4 = new Date(year, 0, 4);
    const weekOffset = ((firstThursday - january4) / 86400000 + 3) / 7;
    const firstMonday = new Date(firstThursday.getTime() - (firstThursday.getDay() - 1) * 86400000);
    const mondayOfWeek = new Date(firstMonday.getTime() + (week - 1) * 7 * 86400000);

    const dates = [];
    for (let i = 0; i < 5; i++) {
        const date = new Date(mondayOfWeek);
        date.setDate(mondayOfWeek.getDate() + i);
        dates.push(date);
    }
    return dates;
}

const dates = getWeekdayDates(year, week);

const weekdays = ['Sonntag', 'Montag', 'Dienstag', 'Mittwoch', 'Donnerstag', 'Freitag', 'Samstag'];
for (let i = 0; i < 5; i++) { // Montag bis Freitag
    const date = dates[i];
    tR += `## ${weekdays[date.getDay()]} ${date.getFullYear()}-${(date.getMonth() + 1).toString().padStart(2, '0')}-${date.getDate().toString().padStart(2, '0')}\n\n`;
}
%>

