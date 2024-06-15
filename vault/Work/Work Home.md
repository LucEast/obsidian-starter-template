---
tags:
  - "#work"
  - dashboard
include_in_navbar: true
navbar_name: Work
cssclasses: []
---
```dataviewjs
let navbar = [];
let loadingMessage = dv.el("span", "**Loading navigation...**", {attr: {style: "font-size:13px; color:gray"}});

let allPages = dv.pages("#dashboard").sort(page => page.file.folder, "asc");
let filteredPages = allPages.filter(p => 
    p.file.tags.values.includes("#dashboard") && p?.include_in_navbar == true
);

for(let page of filteredPages){
    let navItem = '';
    let navName = 'Untitled';
    let navLink = '';

    if(page.navbar_name === undefined){
        navName = page.file.name;
    } else {
        navName = page.navbar_name;
    }
    navLink = page.file.path;

    // Format the nav  item link
    if(dv.current().file.path === page.file.path){
        navItem = `**[[${navLink}|${navName}]]**`
    } else {
        navItem = `[[${navLink}|${navName}]]`
    }

    navbar.push(navItem)
}

dv.paragraph(navbar.join(' | '))

if(filteredPages.values.length > 0){
    loadingMessage.remove();
}
```
```meta-bind-button
label: Open Journal Dashboard 
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: open
    link: "[[Work/Journal/Work journal Dashboard]]"

```

```meta-bind-button
label: + Add weekly work journal
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: templaterCreateNote
    templateFile: _data_/templates/general/Work weekly setup.md
    folderPath: "Work/Journal/Entries"
    fileName: "unnamed"
    openNote: true
```

> [!blank-container]
> ## Open Tasks 📅
> ```dataviewjs
> dv.taskList(dv.pages('"Work" or #work').file.tasks
> .where(t => !t.completed && t.status != "-" &&  !t.text.includes("@frank") &&
> !t.text.includes("#task")
> ))
> ```


### ✏️  Recently Changed
```dataviewjs
function converteTime(time){
	// Convert from ms to minutes
	let convertedTime = ""
	time = time/60000; // time in minutes

	if(time < 60){
		convertedTime = `${Math.ceil(time)} minutes ago`;
	} else if (time < 1440){
		convertedTime = `${Math.ceil(time/60)} hours ago`
	} else {
		convertedTime = `${Math.ceil(time/1440)} days ago`
	}	
	return convertedTime
}

for (let group of dv.pages('"Work"').sort(k => k.file.mtime, 'desc').limit(10).groupBy(p => p.page)) {
	dv.table(["Name", "Date Modified", "Mentions", ""], 
		group.rows
			.sort(k => k.file.mtime, 'desc')
			.map(k => [
			k.file.link, 
			converteTime(Date.now()-k.file.mtime),
			k.file.inlinks.length,
			`<small>[[${k.file.path}|View →]]</small>`
			]))}
```

```dataviewjs
const today = new Date();
const startOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay());
const endOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay() + 6);

dv.list(dv.pages('"Work" or #work').where(p => p.file.ctime >= startOfWeek && p.file.ctime <= endOfWeek).file.link)
  
```

