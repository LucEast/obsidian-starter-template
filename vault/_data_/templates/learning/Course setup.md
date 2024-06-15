<%*
let name = await tp.system.prompt("Course name");
let subject = await tp.system.prompt("Subject (press enter to skip)");
let description = await tp.system.prompt("Description (press enter to skip)");
name = name.trim();
subject = subject.trim();
description = description.trim();

let includeFolderBasedAssignments = await tp.system.suggester(["Yes", "No"], [true, false], false, "Do you want to include folder-based assignments?")

await this.app.vault.createFolder(`Learning/${name}/`);
await this.app.vault.createFolder(`Learning/${name}/Lectures/`);
await this.app.vault.createFolder(`Learning/${name}/Notes/`);

if(includeFolderBasedAssignments == true){
	await this.app.vault.createFolder(`Learning/${name}/Assignments/`);
}

await tp.file.move(`Learning/${name}/` + tp.file.title)
await tp.file.rename(`${name}`);

const filePath = tp.file.path(true);
let fileObject = this.app.vault.getAbstractFileByPath(filePath);
let fileProjectRoot = fileObject.path.split("/")[0] + "/" + fileObject.path.split("/")[1];
_%>
<% "---" %>
document_type: course
tags: sub-dashboard course
completed: false
<% "---" %>
[[Learning Dashboard|Learning Dashboard]] / **<% `[[${filePath}|${fileObject.parent.name}]]` %>**
# <% `${fileObject.parent.name}` %>
**Overview**
Title:: <% `${fileObject.parent.name}` %>
Subject:: <% `${subject}` %>
Description:: <% `${description}` %>
Completed:: `INPUT[toggle:completed]`
Link:: <% tp.file.cursor(1) %>


## Actions
```meta-bind-button
label: + Create Lecture
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: templaterCreateNote
    templateFile: _data_/templates/learning/Course lecture.md
    folderPath: "<% `${fileProjectRoot}` %>/Lectures"
    fileName: "unnamed lecture"
    openNote: true

```
<%* if(includeFolderBasedAssignments == true) { %>
```meta-bind-button
label: + Add Assignment
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: templaterCreateNote
    templateFile: _data_/templates/learning/Course assignment.md
    folderPath: "<% `${fileProjectRoot}` %>/Assignments"
    fileName: "unnamed assignment"
    openNote: true

```
<%* }  %>
```meta-bind-button
label: + Add Note
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: templaterCreateNote
    templateFile: _data_/templates/learning/Course note.md
    folderPath: "<% `${fileProjectRoot}` %>/Notes"
    fileName: "unnamed note"
    openNote: true

```


## Lectures
```dataviewjs
for (let group of dv.pages('"<% `${fileProjectRoot}` %>" and #lecture').groupBy(p => p.lecture)) {
	dv.table(["Lecture", "Description", "Date created"], 
		group.rows 
			.sort(k => k.file.ctime, 'desc')
			.map(k => [
			k.file.link, 
			k['description'],
			k.file.ctime.year+"-"+k.file.ctime.month+"-"+k.file.ctime.day
			]))
}
```


## Assignments
<%* if(includeFolderBasedAssignments == true) { %>
```dataviewjs
for (let group of dv.pages('"<% `${fileProjectRoot}` %>" and #assignment').groupBy(p => p.lecture)) {
	dv.table(["Lecture", "Completed", "Date created"], 
		group.rows 
			.sort(k => k.file.ctime, 'desc')
			.map(k => [
			k.file.link, 
			k['completed'],
			k.file.ctime.year+"-"+k.file.ctime.month+"-"+k.file.ctime.day
			]))
}
```
<%* }  %>

## Notes
```dataviewjs
for (let group of dv.pages('"<% `${fileProjectRoot}` %>" and #course-note').groupBy(p => p.lecture)) {
	dv.table(["Note", "Description"], 
		group.rows 
			.sort(k => k.file.ctime, 'desc')
			.map(k => [
			k.file.link, 
			k['description']
			]))
}
```


---
[[Learning Dashboard|Learning Dashboard]] / **<% `[[${filePath}|${fileObject.parent.name}]]` %>**