---
tags:
  - status/todo
  - work
date_created: 
date_modified: 2023-11-09
---

```dataviewjs
let tasks = dv.pages('"Work" or #work').file.tasks
let completedTasks = dv.pages('"Work" or #work').file.tasks.where(t => t.completed)
let incompletedTasks = dv.pages('"Work" or #work').file.tasks.where(t => !t.completed)
let percentCompleted = (completedTasks.length/tasks.length)*100
 
if (tasks.length > 0) {
  dv.el("div", `
 <progress value="${(completedTasks.length/tasks.length)*100}" max="100"></progress>
 <small>${Math.round((completedTasks.length/tasks.length)*100, 0)}% completed (${incompletedTasks.length} tasks remaining)</small>
 `)
}
```


>[!multi-column]
>> [!blank-container]
>> ## Open Tasks 📅
>> ```dataviewjs
>> dv.taskList(dv.pages('"Work" or #work').file.tasks
>> .where(t => !t.completed && t.status != "-" && !t.text.includes("@frank") &&
>> !t.text.includes("#task")
>> ))
>> ```
>
>> [!blank-container]
>> ## This Weeks Tasks 📅
>> ```dataviewjs
>> const today = new Date();
>> const startOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay());
>> const endOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay() + 6);
>> dv.taskList(dv.pages('"Work" or #work').where(p => p.file.ctime >= startOfWeek && p.file.ctime <= endOfWeek).file.tasks)
>> ```

```dataviewjs
const today = new Date();
const startOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay());
const endOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay() + 6);

dv.taskList(dv.pages('"Work" or #work').where(p => p.file.ctime >= startOfWeek && p.file.ctime <= endOfWeek).file.tasks)
```
```dataviewjs
const today = new Date();
const startOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay());
const endOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay() + 6);

// Sammeln Sie alle relevanten Seiten und deren Aufgaben
let pagesWithTasks = dv.pages('"Work" or #work')
    .where(p => p.file.ctime >= startOfWeek && p.file.ctime <= endOfWeek)
    .flatMap(p => p.file.tasks.map(task => ({ ...task, page: p })))
    .filter(task => task.status && task.status.name); // Stellen Sie sicher, dass nur Aufgaben mit status.name berücksichtigt werden

// Gruppieren Sie die Aufgaben nach status.name
let tasksGroupedByStatus = {};
for (let task of pagesWithTasks) {
    if (!tasksGroupedByStatus[task.status.name]) {
        tasksGroupedByStatus[task.status.name] = [];
    }
    tasksGroupedByStatus[task.status.name].push(task);
}

// Erstellen Sie für jede Gruppe eine eigene Liste und zeigen Sie sie an
Object.keys(tasksGroupedByStatus).forEach(statusName => {
    dv.header(3, statusName); // Verwenden Sie einen h3-Header für den Gruppennamen
    dv.list(tasksGroupedByStatus[statusName].map(task => {
        // Dies hängt von der Struktur Ihrer Aufgaben ab; Sie müssen möglicherweise anpassen, wie die Aufgabenbeschreibung extrahiert wird
        return `${task.text} (${task.page.file.link})`;
    }));
});

```
```tasks
not done
group by status.name
group by created
(path includes Work) OR (tags includes work)
short mode
```


