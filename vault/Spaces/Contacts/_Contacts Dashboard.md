---
aliases: 
tags:
  - dashboard
  - contact
title: _Contacts Dashboard
date created: 07.05.2024 14:02:32
date modified: 07.05.2024 14:02:47
document_type: dashboard
---

# _Contacts Dashboard

Collection of contacts

Contacts: `$= dv.pages('"Contacts" and !#dashboard').length`

**Add a new Contact**

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
for(let group of dv.pages('"Spaces/Contacts" AND !#dashboard').groupBy(p => p.contact)) {
	dv.table(["Name", "Email"], 
		group.rows 
			.sort(k => k.file.name, 'desc')
			.map(k => [
			k.file.link, 
			k['email'],
			]))}
```
