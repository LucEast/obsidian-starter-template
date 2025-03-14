---
banner: "![[banner-learning.jpg]]"
banner_y: 0.65
date_created: 2023-07-06
date_modified: 2023-07-18
description: List of all courses and learning activities.
document_type: dashboard
include_in_navbar: true
navbar_name: Learning
tags: 
- " #dashboard " 
- " #learning "
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
# Learning
Courses, lectures, workshops, and anything related to learning.

```meta-bind-button
style: default
label: + Add course
actions:
  - type: templaterCreateNote
    templateFile: "_data_/templates/learning/Course setup.md"
    folderPath: Learning
    openNote: true
```

### Courses
#### In progress
```dataviewjs
for (let group of dv.pages('"Learning" and #sub-dashboard and #course').groupBy(p => p.note)) {
    let filteredRows = group.rows.filter(k => k['completed'] === false);
    if (filteredRows.length > 0) {
        dv.table(["Course", "Description", "Completed", "Date Created"], 
            filteredRows
                .sort(k => k.file.ctime, 'desc')
                .map(k => [
                "[[" + k.file.path + "|"+ k.file.folder.split('/')[k.file.folder.split('/').length-1] +"]]", 
                k['description'],
                k['completed'],
                k.file.ctime.year+"-"+k.file.ctime.month+"-"+k.file.ctime.day,
                ]));
    } else {
        dv.paragraph("*No courses in progress*");
    }
}
```

#### Completed
```dataviewjs
for (let group of dv.pages('"Learning" and #sub-dashboard and #course').groupBy(p => p.note)) {
    let filteredRows = group.rows.filter(k => k['completed'] === true);
    if (filteredRows.length > 0) {
        dv.table(["Course", "Description", "Completed", "Date Created", "Grade"], 
            filteredRows
                .sort(k => k.file.ctime, 'desc')
                .map(k => [
                "[[" + k.file.path + "|"+ k.file.folder.split('/')[k.file.folder.split('/').length-1] +"]]", 
                k['description'],
                k['completed'],
                k.file.ctime.year+"-"+k.file.ctime.month+"-"+k.file.ctime.day,
                k['grade'],
                ]));
    } else {
        dv.paragraph("*No courses completed*");
    }
}
```

```dataviewjs
// Fetch all pages under the "Learning" path
let pages = dv.pages('"Learning"');

// Filter pages to only include those with `document_type` set to "course" and a defined `grade`
let courses = pages.filter(p => p.document_type && p.document_type === "course" && p.grade !== undefined);

let totalGrades = 0;
let gradeCount = 0;

// Sum up the grades and count the number of graded courses
for (let course of courses) {
    totalGrades += course.grade;
    gradeCount += 1;
}

// Calculate the average grade
let averageGrade = gradeCount > 0 ? totalGrades / gradeCount : 0;

// Display the result
dv.paragraph(`**Average Grade:** ${averageGrade.toFixed(2)}`);
```


### Course Setup Process
To add a new course, click `+ Add course` to setup and add a new course and follow the prompts.

**Course structure**
```
Learning/
└── Example Course/
	├── Assignments/ (note: this will only created if you select "Yes" on folder-based assignments prompt during setup)
	│    └── 01 - My Assignment.md
	├── Lectures/
	│    └── 01 - My First Lecture.md
	├── Notes/
	|    └── 01 - My Note.md
	└── Home.md
```

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