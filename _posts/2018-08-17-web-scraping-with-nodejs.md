---
title: 'Web Scraping with NodeJS and Cheerio'
date: 2018-08-17
permalink: /posts/2018/08/nodejs-web-scraping/
tags:
  - NodeJS
  - Web Scraping
  - Cheerio
  - JavaScript
---

Web Scraping has been of an interest to a lot of businesses and individuals with the immense potential of the quantitative data available online. The data collected can entice the growth of an organization or a personal business. Through this post, we will see through examples on how NodeJS can be used to scrape content from a website. The intentions of this post is to host example code snippets so people can take ideas from it to build scrapers as per their needs using cheerio module in NodeJS. I will be using github's trending page https://github.com/trending throughout this post for the examples, especially because it best suits for applying various cheerio methods.

## Installation

```
npm install --save promise request-promise cheerio
```

## Get html of a page:

```javascript
var Promise = require("promise");
var request = require("request-promise");
var cheerio = require("cheerio");

function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load)
            .then(console.log)
            .then(resolve)
            .then(reject)
    });
}
```

## Using cheerio to get title from a page

```javascript
function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load, requestError)
            .then(scrapeContent)
            .then(resolve)
            .then(reject)
    });
}


function scrapeContent($){
    console.log($('title').text());
}

function requestError() {
    console.log("The trending page could not be loaded!");
    throw Error("Could not fetch html!");

}


getHtml();
```

## Find single element by tag name, find multiple elements by tag name

```javascript
function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load, requestError)
            .then(scrapeContent)
            .then(resolve)
            .then(reject)
    });
}


function scrapeContent($){
    //console.log($);
    console.log($('title').text());
    var all_li_elements = $('ol').find('li');
    all_li_elements.each(function(item){
        console.log($(this).find('h3').text().trim());
    });
    
}

function requestError() {
console.log("The trending page could not be loaded!");
throw Error("Could not fetch html!");
}


getHtml();
```

**Output**

```
Trending repositories on GitHub today · GitHub
Microsoft / FASTER
MichaelMure / git-bug
google / python-fire
Droogans / unmaintainable-code
Avik-Jain / 100-Days-Of-ML-Code
pwxcoo / chinese-xinhua
JuliaLang / julia
r-spacex / SpaceX-API
IEEEKeralaSection / rescuekerala
react-tools / react-move
imhuay / Interview_Notes-Chinese
crossoverJie / JCSprout
benhoyt / goawk
salesforce / TransmogrifAI
Jeffail / benthos
aykevl / tinygo
astorfi / Deep-Learning-World
vuejs / vue
firehol / netdata
loveRandy / vue-cli3.0-vueadmin
trekhleb / javascript-algorithms
palmerhq / react-async-elements
messeb / ios-project-env-setup
jianstm / Schedule
kholia / OSX-KVM
```

## Getting Attributes of an element

```javascript
function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load, requestError)
            .then(scrapeContent)
            .then(resolve)
            .then(reject)
    });
}


function scrapeContent($){
    //console.log($);
    console.log($('title').text());
    var all_li_elements = $('ol').find('li');
    all_li_elements.each(function(item){
        console.log("https://github.com/" + $(this).find('a').attr('href'));
    });
}

function requestError() {
    console.log("The trending page could not be loaded!");
    throw Error("Could not fetch html!");

}


getHtml();
```

**Output**

```
Trending repositories on GitHub today · GitHub
https://github.com//Microsoft/FASTER
https://github.com//MichaelMure/git-bug
https://github.com//google/python-fire
https://github.com//Droogans/unmaintainable-code
https://github.com//Avik-Jain/100-Days-Of-ML-Code
https://github.com//pwxcoo/chinese-xinhua
https://github.com//JuliaLang/julia
https://github.com//r-spacex/SpaceX-API
https://github.com//IEEEKeralaSection/rescuekerala
https://github.com//react-tools/react-move
https://github.com//imhuay/Interview_Notes-Chinese
https://github.com//crossoverJie/JCSprout
https://github.com//benhoyt/goawk
https://github.com//salesforce/TransmogrifAI
https://github.com//Jeffail/benthos
https://github.com//aykevl/tinygo
https://github.com//astorfi/Deep-Learning-World
https://github.com//vuejs/vue
https://github.com//firehol/netdata
https://github.com//loveRandy/vue-cli3.0-vueadmin
https://github.com//trekhleb/javascript-algorithms
https://github.com//palmerhq/react-async-elements
https://github.com//messeb/ios-project-env-setup
https://github.com//jianstm/Schedule
https://github.com//kholia/OSX-KVM
```

## Using class name or other attributes to get element

```javascript
function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load, requestError)
            .then(scrapeContent)
            .then(resolve)
            .then(reject)
    });
}


function scrapeContent($){
    //console.log($);
    console.log($('title').text());
    var all_li_elements = $('ol').find('li');
    all_li_elements.each(function(item){
        console.log($(this).find('.float-sm-right').text().trim());
    });
}

function requestError() {
    console.log("The trending page could not be loaded!");
    throw Error("Could not fetch html!");

}


getHtml();
```

**Output**
```
Trending  repositories on GitHub today · GitHubAsset 1Asset 1
880 stars today
744 stars today
614 stars today
311 stars today
191 stars today
182 stars today
178 stars today
179 stars today
103 stars today
152 stars today
134 stars today
129 stars today
128 stars today
126 stars today
125 stars today
122 stars today
104 stars today
99 stars today
107 stars today
108 stars today
101 stars today
102 stars today
89 stars today
88 stars today
76 stars today
```

## Navigate childrens from an element

```javascript
function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load, requestError)
            .then(scrapeContent)
            .then(resolve)
            .then(reject)
    });
}


function scrapeContent($){
    //console.log($);
    console.log($('title').text());
    var all_li_elements = $('ol').children(); // using children()
    all_li_elements.each(function(item){
        console.log($(this).find('h3').text().trim());
    });
}

function requestError() {
    console.log("The trending page could not be loaded!");
    throw Error("Could not fetch html!");

}


getHtml();
```

**Output**

```
Trending repositories on GitHub today · GitHub
Microsoft / FASTER
MichaelMure / git-bug
google / python-fire
Droogans / unmaintainable-code
Avik-Jain / 100-Days-Of-ML-Code
pwxcoo / chinese-xinhua
JuliaLang / julia
r-spacex / SpaceX-API
IEEEKeralaSection / rescuekerala
react-tools / react-move
imhuay / Interview_Notes-Chinese
crossoverJie / JCSprout
benhoyt / goawk
salesforce / TransmogrifAI
Jeffail / benthos
aykevl / tinygo
astorfi / Deep-Learning-World
vuejs / vue
firehol / netdata
loveRandy / vue-cli3.0-vueadmin
trekhleb / javascript-algorithms
palmerhq / react-async-elements
messeb / ios-project-env-setup
jianstm / Schedule
kholia / OSX-KVM
```

The .children will only return the immediate childrens of the parent element.

## Navigating previous and next siblings of an element

```javascript
function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load, requestError)
            .then(scrapeContent)
            .then(resolve)
            .then(reject)
    });
}


function scrapeContent($){
    //console.log($);
    console.log($('title').text());
    var fifth_li_element = $('ol').find('li').eq(4); // use eq(index) to access via indices.
    var fourth_li_element = fifth_li_element.prev(); // previous sibling
    console.log(fourth_li_element.find('h3').text().trim());
    var sixth_li_element = fifth_li_element.next(); // next sibling
    console.log(sixth_li_element.find('h3').text().trim());
}

function requestError() {
    console.log("The trending page could not be loaded!");
    throw Error("Could not fetch html!");

}


getHtml();
```

## Putting it all together(Github Trending Scraper)

```javascript
function getHtml(){
    return new Promise(function(resolve, reject){
        request("https://github.com/trending")
            .then(cheerio.load, requestError)
            .then(scrapeContent)
            .then(resolve)
            .then(reject)
    });
}


function scrapeContent($){
    //console.log($);
    var all_li_elements = $('ol').find('li');
    all_li_elements.each(function(item){
        repository_name = $(this).find('h3').text().trim();
        total_stars_today = $(this).find('.float-sm-right').text().trim();
        repository_url = 'https://github.com/' + $(this).find('a').attr('href');
        console.log(repository_name, "\t", total_stars_today, "\t", repository_url);
    });
}

function requestError() {
    console.log("The trending page could not be loaded!");
    throw Error("Could not fetch html!");

}

getHtml();
```

**Output**
```
Microsoft / FASTER                946 stars today      https://github.com//Microsoft/FASTER
google / python-fire              647 stars today      https://github.com//google/python-fire
MichaelMure / git-bug             578 stars today      https://github.com//MichaelMure/git-bug
Droogans / unmaintainable-code    228 stars today      https://github.com//Droogans/unmaintainable-code
pwxcoo / chinese-xinhua           198 stars today      https://github.com//pwxcoo/chinese-xinhua
Avik-Jain / 100-Days-Of-ML-Code   188 stars today      https://github.com//Avik-Jain/100-Days-Of-ML-Code
JuliaLang / julia                 170 stars today      https://github.com//JuliaLang/julia
aykevl / tinygo                   169 stars today      https://github.com//aykevl/tinygo
IEEEKeralaSection / rescuekerala  96 stars today       https://github.com//IEEEKeralaSection/rescuekerala
benhoyt / goawk                   152 stars today      https://github.com//benhoyt/goawk
r-spacex / SpaceX-API             150 stars today      https://github.com//r-spacex/SpaceX-API
crossoverJie / JCSprout           129 stars today      https://github.com//crossoverJie/JCSprout
imhuay / Interview_Notes-Chinese  121 stars today      https://github.com//imhuay/Interview_Notes-Chinese
trekhleb / javascript-algorithms  113 stars today      https://github.com//trekhleb/javascript-algorithms
salesforce / TransmogrifAI        107 stars today      https://github.com//salesforce/TransmogrifAI
loveRandy / vue-cli3.0-vueadmin   110 stars today      https://github.com//loveRandy/vue-cli3.0-vueadmin
astorfi / Deep-Learning-World     102 stars today      https://github.com//astorfi/Deep-Learning-World
vuejs / vue                       97 stars today       https://github.com//vuejs/vue
Jeffail / benthos                 104 stars today      https://github.com//Jeffail/benthos
palmerhq / react-async-elements   92 stars today       https://github.com//palmerhq/react-async-elements
firehol / netdata                 89 stars today       https://github.com//firehol/netdata
jesseduffield / lazygit           84 stars today       https://github.com//jesseduffield/lazygit
jianstm / Schedule                75 stars today       https://github.com//jianstm/Schedule
shadowsocks/shadowsocks-windows   67 stars today       https://github.com//shadowsocks/shadowsocks-windows
mawww / kakoune                   71 stars today       https://github.com//mawww/kakoune
```

There is another version of this article using same set of examples using BeautifulSoup in Python. You should read it too. https://www.thetaranights.com/web-scraping-beautifulsoup-python/