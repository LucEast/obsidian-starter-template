---
banner: "![[banner-books.jpg]]"
date_created: 2023-07-08
date_modified: 2023-07-18
description: List of all books and notes.
document_type: dashboard
tags:
  - dashboard
  - books
cssclasses: []
---

[[Games Dashboard|Spaces]] / **[[Spaces/Books/Books Dashboard|Books]]**

# Books

Collection of books and book notes.

Books read: `$= dv.pages('"Books" and !#dashboard').where(page => page.status =="read").length`

Books currently reading: `$= dv.pages('"Books" and !#dashboard').where(page => page.status =="unread").length`

Total number of books: `$= dv.pages('"Books" and !#dashboard').length`

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
for (let group of dv.pages('"Spaces/Books/Entries" and !#dashboard').groupBy(p => p.book)) {
	dv.table(["Image", "Title", "Category", "Read"], 
		group.rows 
			.sort(k => k.file.ctime, 'desc')
			.map(k => [
			("![|100](" + k['image'] + ")"),
			k.file.link, 
			k['category'],
			k['read']
			]))}
```

---

[[Games Dashboard|Spaces]] / **[[Spaces/Books/Books Dashboard|Books]]**
