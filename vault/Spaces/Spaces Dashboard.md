---
banner: "![[banner-spaces.jpg]]"
date_created: 2023-07-06
date_modified: 2023-07-18
description: Collection of other spaces
document_type: dashboard
include_in_navbar: true
navbar_name: Spaces
tags:
  - dashboard
  - spaces
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

# Spaces

```meta-bind-button
label: + Add Space
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: templaterCreateNote
    templateFile: _data_/templates/spaces/Space home.md
    folderPath: "Spaces"
    fileName: "unnamed space"
    openNote: true

```

## Dashboards

```dataviewjs
for (let group of dv.pages('"Spaces" and #dashboard and !"${dv.current().file.path}"').groupBy(p => p.note)) {
	dv.table(["Name", "Description", "Tags"], 
		group.rows 
			.sort(k => k.file.name, 'asc')
			.map(k => [
			k.file.link, 
			k['description'],
			dv.span(k.file.tags.values.toString().replaceAll(",", " "))
			]))}
```

---

## Books

Collection of books and book notes.

Books read: `$= dv.pages('"Spaces/Books" and !#dashboard').where(page => page.status =="read").length`

Books currently reading: `$= dv.pages('"Spaces/Books" and !#dashboard').where(page => page.status =="unread").length`

Total number of books: `$= dv.pages('"Spaces/Books" and !#dashboard').length`

**Add a new book**

```meta-bind-button
label: + Add Book
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: command
    command: obsidian-media-db-plugin:open-media-db-search-modal-with-book

```

**Table of books**

```dataviewjs
for (let group of dv.pages('"Spaces/Books" and !#dashboard').groupBy(p => p.book)) {
	dv.table(["Cover", "Title", "Category", "Status"], 
		group.rows 
			.sort(k => k.file.ctime, 'desc')
			.map(k => [
			("![|100](" + k['cover'] + ")"),
			k.file.link, 
			k['category'],
			k['status']
			]))}
```


---

## Movies/Series

Collection of movies and series.

Movies/Series watched: `$= dv.pages('"Spaces/Movies/Entries" and !#dashboard').where(page => page.watched ==true).length`

Total number of movies/series: `$= dv.pages('"Spaces/Movies/Entries" and !#dashboard').length`

**Add a new movie**/series

```meta-bind-button
label: + Add Movie
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: command
    command: obsidian-media-db-plugin:open-media-db-search-modal-with-movie
```

```meta-bind-button
label: + Add Series
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: command
    command: obsidian-media-db-plugin:open-media-db-search-modal-with-series
```

**Table of movies**

```dataview
table without id 
	file.link as Title,
	string(year) as Year, 
	"by " + director as Director,
	"⭐ " + scoreImdb as "⭐ IMDB",
	rating
from #mediaDB/tv/movie or #mediaDB/tv/series  
where image != null and file.name != "movie template"
```

---

## Games

Collection of Games.

Games played: `$= dv.pages('"Spaces/Games/Entries" and !#dashboard').where(page => page.played ==true).length`

Total number of movies/series: `$= dv.pages('"Spaces/Games/Entries" and !#dashboard').length`

**Add a new game

```meta-bind-button
label: + Add Game
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: command
    command: obsidian-media-db-plugin:open-media-db-search-modal-with-game
```

**Table of games**

```dataview
table without id 
	file.link as Title,
	string(year) as Year, 
	genres as Genres,
	onlineRating,
	"played: " + played as Played
from #mediaDB/game 
where image != null and file.name != "game template"
```

---

## Contacts

```meta-bind-button
label: + Add Contact
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: templaterCreateNote
    templateFile: _data_/templates/general/Contact.md
    folderPath: Spaces/Contacts
    fileName: unnamed contact
    openNote: true

```

**Table of contacts**

```dataviewjs
for(let group of dv.pages('"Spaces/Contacts"').groupBy(p => p.contact)) {
	dv.table(["Name", "Email"], 
		group.rows 
			.sort(k => k.file.name, 'desc')
			.map(k => [
			k.file.link, 
			k['email'],
			]))}
```

```dataviewjs
for (let group of dv.pages('"Spaces" and !#dashboard').groupBy(p => p.file.folder)) {
  let headerLevel = group.key.split("/").length
  dv.header(headerLevel, `${group.key}`);
  dv.table(["Name", "Description", "Modified"],
  group.rows.map(k => [k.file.link, k.file['description'], k.file.mtime]))
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
