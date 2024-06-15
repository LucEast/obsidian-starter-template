---
aliases: 
banner: "![[banner-dashboard.jpg]]"
banner_title: Home
banner_icon: 🏠 
tags:
  - " dashboard "
alias:
  - homepage
include_in_navbar: true
navbar_name: Dashboard
banner_y: 0.5
title: Dashboard
date created: 25.05.2023 23:29:39
date modified: 15.06.2024 20:16:47
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

# Dashboard

>[!multi-column]  
>>[!blank-container]
>>## 🏠 Navigation
>>[[Concept Board|💡  Concept Board →]]  
>>[[Journal Dashboard|📘 Journal →]]  
>>[[Learning Dashboard|🎓  Learning →]]  
>>[[!Notes Dashboard|🗒️  Notes →]]  
>>[[Projects/Projects|📐  Projects →]]  
>>[[Personal Dashboard|🔒  Personal →]]  
>>[[Resources Dashboard|ℹ️  Resources →]]  
>>[[Spaces Dashboard|📦  Spaces →]]  
>>[[Work Home|💼 Work Dashboard →]]
>
>>[!blank-container]
>>## 📐 Projects (`$=dv.pages('"Projects" and #project').length`)
>>```dataviewjs
>>let projectList = [];
>>let projects = dv.pages('"Projects" and #dashboard and !#projects');
>>projects = projects.filter(obj => obj.is_active === true);
>>for(let i=0; i<projects.length; i++){
>>	projectList.push(`[[${projects[i].file.path}|${projects[i].file.path.split('/')[projects[i].file.path.split('/').length-2]} →]]`)
>>}
>>dv.list(projectList)
>>```
>
>>[!blank-container]
>>## Open Tasks 📅
>>```tasks
>>not done
>>group by status.name
>>group by created
>>NOT (path includes Work) OR (tags includes work)
>>short mode
>>``` 
>>

## ✏️ Recently Changed

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

for (let group of dv.pages('!"_data_"').sort(k => k.file.mtime, 'desc').limit(10).groupBy(p => p.page)) {
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

**Stats**

Number of files: `$=dv.pages('!"_data_"').length`

Number of notes: `$=dv.pages('"Notes" and !#dashboard').length`

Number of concepts: `$=dv.pages('"Concept Board" and !#dashboard').length`

Total Vault files: `$=dv.pages().length`

Total Vault size: 

---
>[!multi-column]
>>[!blank-container]
>>### 🚀 Upcoming Launches
>>```dataviewjs
>>function construct_elements(data) {
>>for(let i=0; i<data.length; i++){
>>	let missions = [];
>>	for(let j=0; j<data[i].missions.length; j++){missions.push(data[i].missions[j].name)}
>>	dv.el("div", 
>>	`<div class="tailwind"><div class='block py-2'><div class="text-xl font-bold text-gray-800">${data[i].name}<small class="float-right text-gray-600 font-semibold text-sm pl-2">${data[i].date_str}</small></div> <div class="text-sm font-semibold text-gray-600">Vehicle: ${data[i].vehicle.name}</div> <div class="text-sm font-semibold text-gray-600">Missions: ${missions}</div> <div class="text-sm text-gray-600"> ${data[i].launch_description} </div> </div> </div>
>>	`
>>	)
>>}
>>}
>>let response = await fetch('https://fdo.rocketlaunch.live/json/launches/next/5') 
>>response.json().then(j => construct_elements(j.result))
>>```
>
>>[!blank-container]
>>### &emsp;🛰️Space Image of the Day
>>```dataviewjs
>>const {NASAImageOfTheDay} = customJS;
>>let element = this.container.createEl('div', {cls: ["tailwind"]});
>>let apiKey = "PSrdswa5IKTJeSc007kTh8Vg6Y4A8mrxyIIHTeGp";
>>await NASAImageOfTheDay.getImage(element, apiKey);
>>```

## 📰 Recent News

### Stocks

```dataviewjs
const {News} = customJS;
let element = this.container.createEl('div', {cls: ["tailwind"]});
let newsCategory = 'stocks';
let newslang = 'de';
let newsCountry = 'de';
let articleCount = '4';
let apiKey = 'e40a3e9b49a4248f96e15459daa4a434';
await News.listNews(element, newsCategory, newslang, newsCountry, articleCount, apiKey);
```

### Technologie

```dataviewjs
const {News} = customJS;
let element = this.container.createEl('div', {cls: ["tailwind"]});
let newsCategory = 'technology';
let newslang = 'de';
let newsCountry = 'de';
let articleCount = '4';
let apiKey = 'e40a3e9b49a4248f96e15459daa4a434';
await News.listNews(element, newsCategory, newslang, newsCountry, articleCount, apiKey);
```

---
```dataviewjs
const {Navbar} = customJS;
await Navbar.createNavbar(app, dv); 
```
