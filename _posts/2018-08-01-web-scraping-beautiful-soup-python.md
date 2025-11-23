---
title: 'Web Scraping - BeautifulSoup in Python'
date: 2018-08-01
permalink: /posts/2018/08/web-scraping-beautifulsoup-in-python/
tags:
  - Python
  - Web Scraping
  - BeautifulSoup
  - Data Collection
---

Data collection from public sources is often beneficial to a business or an individual. As such the term "web scraping" isn't something new. These data are often wrangled within html tags and attributes. Python is often used for data collection from these sources. The intentions of this post is to host example code snippets so people can take ideas from it to build scrapers as per their needs using BeautifulSoup and urllib module in Python. I will be using github's trending page https://github.com/trending throughout this post for the examples, especially because it best suits for applying various BeautifulSoup methods.

## Installation:
```
pip install BeautifulSoup4
```

## Get html of a page:
```python
>>> import urllib
>>> resp = urllib.request.urlopen("https://github.com/trending")
>>> resp.getcode()
200
>>> resp.read() # the html
```

## Using BeautifulSoup to get title from a page
```python
>>> import urllib
>>> import bs4
>>> github_trending = urllib.request.urlopen("https://github.com/trending")
>>> trending_soup = bs4.BeautifulSoup(github_trending.read(), "lxml")
>>> trending_soup.title
<title>Trending  repositories on GitHub today · GitHub</title>
>>> trending_soup.title.string
'Trending  repositories on GitHub today · GitHub'
>>>
```

## Find single element by tag name, find multiple elements by tag name
```python
>>> ordered_list = trending_soup.find('ol') #single element
>>>
>>> type(ordered_list)
<class 'bs4.element.Tag'>
>>>
>>> all_li = ordered_list.find_all('li') # multiple elements
>>>
>>> type(all_li)
<class 'bs4.element.ResultSet'>
>>>
>>> trending_repositories = [each_list.find('h3').text for each_list in all_li]
>>> for each_repository in trending_repositories:
...     print(each_repository.strip())
...
klauscfhq / taskbook
robinhood / faust
Avik-Jain / 100-Days-Of-ML-Code
jxnblk / mdx-deck
faressoft / terminalizer
trekhleb / javascript-algorithms
apexcharts / apexcharts.js
grain-lang / grain
thedaviddias / Front-End-Performance-Checklist
istio / istio
CyC2018 / Interview-Notebook
fivethirtyeight / russian-troll-tweets
boyerjohn / rapidstring
donnemartin / system-design-primer
awslabs / aws-cdk
QUANTAXIS / QUANTAXIS
crossoverJie / Java-Interview
GoogleChromeLabs / ndb
dylanbeattie / rockstar
vuejs / vue
sbussard / canvas-sketch
Microsoft / vscode
flutter / flutter
tensorflow / tensorflow
Snailclimb / Java-Guide
>>>
```

## Getting Attributes of an element
```python
>>> for each_list in all_li:
...     anchor_element = each_list.find('a')
...     print("https://github.com" + anchor_element['href'])
...
https://github.com/klauscfhq/taskbook
https://github.com/robinhood/faust
https://github.com/Avik-Jain/100-Days-Of-ML-Code
https://github.com/jxnblk/mdx-deck
https://github.com/faressoft/terminalizer
https://github.com/trekhleb/javascript-algorithms
https://github.com/apexcharts/apexcharts.js
https://github.com/grain-lang/grain
https://github.com/thedaviddias/Front-End-Performance-Checklist
https://github.com/istio/istio
https://github.com/CyC2018/Interview-Notebook
https://github.com/fivethirtyeight/russian-troll-tweets
https://github.com/boyerjohn/rapidstring
https://github.com/donnemartin/system-design-primer
https://github.com/awslabs/aws-cdk
https://github.com/QUANTAXIS/QUANTAXIS
https://github.com/crossoverJie/Java-Interview
https://github.com/GoogleChromeLabs/ndb
https://github.com/dylanbeattie/rockstar
https://github.com/vuejs/vue
https://github.com/sbussard/canvas-sketch
https://github.com/Microsoft/vscode
https://github.com/flutter/flutter
https://github.com/tensorflow/tensorflow
https://github.com/Snailclimb/Java-Guide
>>>
```

## Using class name or other attributes to get element
```python
>>> for each_list in all_li:
...     total_stars_today = each_list.find(attrs={'class':'float-sm-right'}).text
...     print(total_stars_today.strip())
...
1,063 stars today
846 stars today
596 stars today
484 stars today
459 stars today
429 stars today
443 stars today
366 stars today
330 stars today
282 stars today
182 stars today
190 stars today
200 stars today
190 stars today
166 stars today
164 stars today
144 stars today
158 stars today
157 stars today
144 stars today
144 stars today
142 stars today
132 stars today
101 stars today
108 stars today
>>>
```

## Navigate childrens from an element
```python
>>> for each_children in ordered_list.children:
...     print(each_children.find('h3').text.strip())
...
klauscfhq / taskbook
robinhood / faust
Avik-Jain / 100-Days-Of-ML-Code
jxnblk / mdx-deck
faressoft / terminalizer
trekhleb / javascript-algorithms
apexcharts / apexcharts.js
grain-lang / grain
thedaviddias / Front-End-Performance-Checklist
istio / istio
CyC2018 / Interview-Notebook
fivethirtyeight / russian-troll-tweets
boyerjohn / rapidstring
donnemartin / system-design-primer
awslabs / aws-cdk
QUANTAXIS / QUANTAXIS
crossoverJie / Java-Interview
GoogleChromeLabs / ndb
dylanbeattie / rockstar
vuejs / vue
sbussard / canvas-sketch
Microsoft / vscode
flutter / flutter
tensorflow / tensorflow
Snailclimb / Java-Guide
>>>
```

The .children will only return the immediate childrens of the parent element. If you'd like to get all of the elements under certain element, you should use .descendent

## Navigate descendents from an element
```python
>>> for each_children in ordered_list.descendent:
...     # perform operations
```

## Navigating previous and next siblings of elements
```python
>>> all_li = ordered_list.find_all('li')
>>> fifth_li = all_li[4]
>>> # each li element is separated by '\n' and hence to navigate to the fourth li, we should navigate previous sibling twice
...
>>>
>>> fourth_li = fifth_li.previous_sibling.previous_sibling
>>> fourth_li.find('h3').text.strip()
'jxnblk / mdx-deck'
>>>
>>> # similarly for navigating to the sixth li from fifth li, we would use next_sibling
...
>>> sixth_li = fifth_li.next_sibling.next_sibling
>>> sixth_li.find('h3').text.strip()
'trekhleb / javascript-algorithms'
>>>
```

## Navigate to parent of an element
```python
>>> all_li = ordered_list.find_all('li')
>>> first_li = all_li[0]
>>> li_parent = first_li.parent
>>> # the li_parent is the ordered list <ol>
...
>>>
```

## Putting it all together(Github Trending Scraper)
```python
>>> import urllib
>>> import bs4
>>>
>>> github_trending = urllib.request.urlopen("https://github.com/trending")
>>> trending_soup = bs4.BeautifulSoup(github_trending.read(), "lxml")
>>> ordered_list = trending_soup.find('ol')
>>> for each_list in ordered_list.find_all('li'):
...     repository_name = each_list.find('h3').text.strip()
...     repository_url = "https://github.com" + each_list.find('a')['href']
...     total_stars_today = each_list.find(attrs={'class':'float-sm-right'}).text
…        print(repository_name, repository_url, total_stars_today)

klauscfhq / taskbook                             https://github.com/klauscfhq/taskbook                             1,404 stars today
robinhood / faust                                https://github.com/robinhood/faust                                960 stars today
Avik-Jain / 100-Days-Of-ML-Code 	         https://github.com/Avik-Jain/100-Days-Of-ML-Code                  566 stars today
trekhleb / javascript-algorithms 	         https://github.com/trekhleb/javascript-algorithms                 431 stars today
jxnblk / mdx-deck 			         https://github.com/jxnblk/mdx-deck 	                           416 stars today
apexcharts / apexcharts.js 		         https://github.com/apexcharts/apexcharts.js 	                   411 stars today
faressoft / terminalizer 		         https://github.com/faressoft/terminalizer 	                   406 stars today
istio / istio 			                 https://github.com/istio/istio 	                           309 stars today
thedaviddias / Front-End-Performance-Checklist 	 https://github.com/thedaviddias/Front-End-Performance-Checklist   315 stars today
grain-lang / grain 			         https://github.com/grain-lang/grain 	                           301 stars today
boyerjohn / rapidstring 			 https://github.com/boyerjohn/rapidstring 	                   232 stars today
CyC2018 / Interview-Notebook 			 https://github.com/CyC2018/Interview-Notebook 	                   186 stars today
donnemartin / system-design-primer 		 https://github.com/donnemartin/system-design-primer 	           189 stars today
awslabs / aws-cdk 			         https://github.com/awslabs/aws-cdk 	                           186 stars today
fivethirtyeight / russian-troll-tweets 		 https://github.com/fivethirtyeight/russian-troll-tweets 	   159 stars today
GoogleChromeLabs / ndb 			         https://github.com/GoogleChromeLabs/ndb 	                   172 stars today
crossoverJie / Java-Interview 			 https://github.com/crossoverJie/Java-Interview 	           148 stars today
vuejs / vue 			                 https://github.com/vuejs/vue 	                                   137 stars today
Microsoft / vscode 			         https://github.com/Microsoft/vscode 	                           137 stars today
flutter / flutter 			         https://github.com/flutter/flutter 	                           132 stars today
QUANTAXIS / QUANTAXIS 			         https://github.com/QUANTAXIS/QUANTAXIS 	                   132 stars today
dylanbeattie / rockstar 			 https://github.com/dylanbeattie/rockstar 	                   130 stars today
tensorflow / tensorflow 			 https://github.com/tensorflow/tensorflow 	                   106 stars today
Snailclimb / Java-Guide 			 https://github.com/Snailclimb/Java-Guide 	                   111 stars today
WeTransfer / WeScan 			         https://github.com/WeTransfer/WeScan 	                           118 stars today
```