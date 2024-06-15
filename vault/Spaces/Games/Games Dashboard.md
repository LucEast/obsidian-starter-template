---
aliases: []
tags:
  - dashboard
  - mediaDB/game
description: list of all games
cssclasses:
  - cards
  - table-max
  - cards-cover
  - cards-2-3
title: Games Dashboard
date created: 17.02.2024 20:25:21
date modified: 18.05.2024 22:29:30
---

# Games Dashboard

[[Games Dashboard|Spaces]] / **[[Games Dashboard|Games]]**

Games

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
	("![](" + image + ")") as Image,
	file.link as Title,
	string(year) as Year, 
	genres as Genres,
	onlineRating,
	"played: " + played as Played
from #mediaDB/game 
where image != null and file.name != "game template"
```

---

[[Games Dashboard|Spaces]] / **[[Games Dashboard|Games]]**
