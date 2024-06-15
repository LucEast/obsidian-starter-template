---
document_type: dashboard
tags:
  - dashboard
---
```simple-time-tracker
{"entries":[{"name":"WSP","startTime":null,"endTime":null,"subEntries":[{"name":"Part 1","startTime":"2024-03-11T07:29:54.000Z","endTime":"2024-03-11T10:50:59.000Z","subEntries":null},{"name":"Part 2","startTime":"2024-03-11T11:40:02.000Z","endTime":"2024-03-11T13:16:02.000Z","subEntries":null},{"name":"Meeting (Hardwareerweiterung)","startTime":"2024-03-18T14:02:14.000Z","endTime":"2024-03-18T14:52:16.000Z","subEntries":null},{"name":"Server beantragen und Installieren","startTime":null,"endTime":null,"subEntries":[{"name":"IP beantragen","startTime":"2024-03-19T07:32:16.000Z","endTime":"2024-03-19T07:37:20.000Z","subEntries":null},{"name":"Grundinstallation wrk04","startTime":"2024-03-19T11:37:37.000Z","endTime":"2024-03-19T12:06:39.000Z","subEntries":null}]},{"name":"Deployment und Bug suche","startTime":null,"endTime":null,"subEntries":[{"name":"Part 1","startTime":"2024-03-22T07:03:55.000Z","endTime":"2024-03-22T07:15:58.000Z","subEntries":null},{"name":"Bug suche","startTime":"2024-03-22T10:45:20.000Z","endTime":"2024-03-22T11:48:23.000Z","subEntries":null}]},{"name":"Deployment","startTime":null,"endTime":null,"subEntries":[{"name":"Part 1","startTime":"2024-03-25T13:42:43.000Z","endTime":"2024-03-25T13:49:47.000Z","subEntries":null},{"name":"Part 2","startTime":"2024-03-26T10:37:43.000Z","endTime":"2024-03-26T13:50:46.000Z","subEntries":null}]},{"name":"Deployment","startTime":"2024-04-10T12:27:52.777Z","endTime":"2024-04-10T12:56:51.363Z","subEntries":null},{"name":"Deployment","startTime":"2024-04-11T10:39:30.891Z","endTime":"2024-04-11T13:09:44.997Z","subEntries":null},{"name":"WSP bug search","startTime":"2024-04-11T13:10:17.462Z","endTime":"2024-04-11T13:57:43.483Z","subEntries":null}]}]}
```

# Journal Entries
Explore your collection of journal entries.

```meta-bind-button
label: + Add work journal entry
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: command
    command: quickadd:choice:0e01a20b-5cab-4964-b8c0-0de1752922af

```

```meta-bind-button
label: + Add weekly note
hidden: false
class: ""
tooltip: ""
id: ""
style: default
actions:
  - type: command
    command: quickadd:choice:58ffce94-c41b-4cf4-9233-291e678d748d

```


**Table of journal entries**
```dataviewjs
for (let group of dv.pages('"Work/OWL-IT/Journal" and !#dashboard').groupBy(p => p.entry)) {
	dv.table(["Entry", "Significant event"], 
		group.rows 
			.sort(k => k.name, 'desc')
			.map(k => [
			k.file.link,
			k['event']
			]))}
```
`$="<small>Number of entries: " + dv.pages('"Work/OWL-IT/Journal/Entries"').length+"</small>"`

```tasks 
done this week
group by done
(path includes Work) OR (tags includes work)
short mode
```


```tasks 
done last week
group by done
(path includes Work) OR (tags includes work)
short mode
```



