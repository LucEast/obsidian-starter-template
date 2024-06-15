<%*
	const filePath = tp.file.path(true);
	let fileObject = this.app.vault.getAbstractFileByPath(filePath);
_%>
<% "---" %>
date_created: <% tp.file.creation_date() %>
date_modified: <% tp.file.last_modified_date() %>
document_type: journal
tags: 
- " #category/daily  " 
<% "---" %>

```dataviewjs
/*
    previous/next note by date for Daily Notes
    Also works for other files having a `date:` YAML entry.
    MCH 2021-06-14
*/
var none = '(none)';
var p = dv.pages('"' + dv.current().file.folder + '"').where(p => p.file.day).map(p => [p.file.name, p.file.day.toISODate()]).sort(p => p[1]);
var t = dv.current().file.day ? dv.current().file.day.toISODate() : luxon.DateTime.now().toISODate();
// Obsidian uses moment.js; Luxon’s format strings differ!
var format = app['internalPlugins']['plugins']['daily-notes']['instance']['options']['format'] || 'YYYY-MM-DD';
var current = '(' + moment(t).format(format) + ')';
var nav = [];
var today = p.find(p => p[1] == t);
var next = p.find(p => p[1] > t);
var prev = undefined;
p.forEach(function (p, i) {
    if (p[1] < t) {
        prev = p;
    }
});
nav.push(prev ? '[[' + prev[0] + ']]' : none);
//nav.push(today ? today[0] : none);
nav.push(today ? today[0] : current);
nav.push(next ? '[[' + next[0] + ']]' : none);

//dv.list(nav);
//dv.paragraph(nav.join(" · "));
dv.paragraph(nav[0] + ' ← ' + nav[1] + ' → ' + nav[2]);
```

[[Journal/Journal Dashboard|Journal]] / **<% `[[${filePath.slice(0,-3)}|${fileObject.basename}]]` %>**
<% `# ${fileObject.basename}` %>

> [!important] Significant Event
> Event:: 

> [!quote]+
<% tp.web.daily_quote() %>

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

>[!Notes] 

<% tp.file.cursor(1) %>

---
[[Journal/Journal Dashboard|Journal]] / **<% `[[${filePath.slice(0,-3)}|${fileObject.basename}]]` %>**