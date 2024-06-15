---
tags:
  - dashboard
  - mediaDB/tv/series
  - mediaDB/tv/movie
description: list of all movies and series
cssclasses:
  - table-max
  - cards
  - cards-2-3
  - cards-cover
---

[[Games Dashboard|Spaces]] / **[[Movies Dashboard|Movies]]**

# Movies/Series

Collection of movies and series.

Movies/Series watched: `$= dv.pages('"Spaces/Movies/Entries" and !#dashboard').where(page => page.watched ==true).length`

Total number of movies/series: `$= dv.pages('"Spaces/Movies/Entries" and !#dashboard').length`

**Add a new movie**

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
	("![](" + image + ")") as Image,
	file.link as Title,
	string(year) as Year, 
	"by " + writer as Writer,
	genres as Genres,
	actors as Actors,
	onlineRating as Rating,
	"watched: " + watched as Watched
from #mediaDB/tv/movie or #mediaDB/tv/series  
where image != null and file.name != "movie template"
```

---

[[Games Dashboard|Spaces]] / **[[Movies Dashboard|Movies]]**
