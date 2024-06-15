---
aliases: 
banner: ""
date_created: 2023-07-08
date_modified: 2023-07-18
description: List of all travel and trips.
document_type: dashboard
tags:
  - dashboard
  - travel
  - trips
cssclasses: []
title: Trips
date created: 24.07.2023 10:06:54
date modified: 14.04.2024 7:31:17
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

# Trips

List of all travel and trips.

```meta-bind-button
label: + Add trip
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: templaterCreateNote
    templateFile: _data_/templates/general/Travel and Trip.md
    folderPath: Spaces/Trips/Entries
    fileName: unnamed trip
    openNote: true

```

## Planned Trips

```dataviewjs
let currentPath = dv.current().file.folder + "/Entries";

if(dv.pages(`"${currentPath}"`).length > 0){
    for(let group of dv.pages(`"${currentPath}"`).groupBy(p => p.concepts)) {
        let filteredRows = group.rows.filter(k => k['completed'] === false);
        if (filteredRows.length > 0) {
            dv.table(["Name", "Destination"], 
                filteredRows
                    .sort(k => k.file.ctime, 'desc')
                    .map(k => [
                    k.file.link,
                    k['destination']
                    ]));
        } else {
            dv.paragraph('*No planned trips*')
        }
    }
} else {
    dv.paragraph('*No planned trips*')
}
```

## Completed Trips

```dataviewjs
let currentPath = dv.current().file.folder + "/Entries";

if(dv.pages(`"${currentPath}"`).length > 0){
    for(let group of dv.pages(`"${currentPath}"`).groupBy(p => p.concepts)) {
        let filteredRows = group.rows.filter(k => k['completed'] === true);
        if (filteredRows.length > 0) {
            dv.table(["Name", "Destination"], 
                filteredRows
                    .sort(k => k.file.ctime, 'desc')
                    .map(k => [
                    k.file.link,
                    k['destination']
                    ]));
        } else {
            dv.paragraph('*No completed trips*')
        }
    }
} else {
    dv.paragraph('*No completed trips*')
}
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
