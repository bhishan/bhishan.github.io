---
title: 'Web Scraping with Golang and Goquery'
date: 2018-09-13
permalink: /posts/2018/09/golang-web-scraping/
tags:
  - Golang
  - Web Scraping
  - Goquery
  - Tutorials
---

Web Scraping can be beneficial to individuals and companies. The intentions of this post is to host a set of examples on Web Scraping using Golang and goquery. I will be using github's trending page https://github.com/trending throughout this post for the examples, especially because it best suits for applying various goquery methods. There are two other versions of this article which replicates the same set of examples in Python and NodeJS.

## Installation

```
go get github.com/PuerkitoBio/goquery
```

## Get html of a page

```go
package main
import (
    "log"
    "io"
    "os"
    "net/http"
)

func ScrapeHTML(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()

    if resp.StatusCode != 200{
        log.Fatalf("status code error: %d %s", resp.StatusCode, resp.Status)
    }
    io.Copy(os.Stdout, resp.Body)    
}

func main(){
    ScrapeHTML()
}
```

## Using goquery(golang library) to get title from a page

```go
package main
import (
    "fmt"
    "log"
    "net/http"
    "github.com/PuerkitoBio/goquery"
)

func ScrapeHTML(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()

    if resp.StatusCode != 200{
        log.Fatalf("status code error: %d %s", resp.StatusCode, resp.Status)
    }

    doc, err := goquery.NewDocumentFromReader(resp.Body)
  if err != nil {
        log.Fatal(err)
    }
    fmt.Println(doc.Find("title").Text())
   
}

func main(){
    ScrapeHTML()
}
```

**Output**
```
$ go run example.go
Trending repositories on GitHub today · GitHub
```

## Using goquery, Find single element by tag name, find multiple elements by tag name

```go
package main
import (
    "fmt"
    "log"
    "strings"
    "net/http"
    "github.com/PuerkitoBio/goquery"
)


func scrapeUsingTagNames(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()

    if resp.StatusCode != 200{
        log.Fatalf("Status code error: %d %s", resp.StatusCode, resp.Status)
    }
    doc, err := goquery.NewDocumentFromReader(resp.Body)
    if err != nil{
        log.Fatal(err)
    }
    fmt.Println(doc.Find("title").Text())

    doc.Find("ol li").Each(func(i int, s *goquery.Selection){
        fmt.Println(strings.TrimSpace(s.Find("h3").Text()))
    })
}

func main(){
    scrapeUsingTagNames()
}
```

**Output**
```
$ go run example.go
Trending repositories on GitHub today · GitHubAsset 1Asset 1
you-dont-need / You-Dont-Need-Momentjs
ripienaar / free-for-dev
Nozbe / WatermelonDB
cjbarber / ToolsOfTheTrade
byoungd / English-level-up-tips-for-Chinese
TheAlgorithms / Python
thedaviddias / Front-End-Checklist
zziz / pwc
dawnlabs / carbon
CyC2018 / CS-Notes
Avik-Jain / 100-Days-Of-ML-Code
donnemartin / system-design-primer
mariusandra / pigeon-maps
Snailclimb / JavaGuide
JavaNoober / BackgroundLibrary
crossoverJie / JCSprout
Microsoft / nni
PansonPanson / Java-Notes
date-fns / date-fns
sindresorhus / ky
mciastek / sal
rwv / chinese-dos-games
vuejs / vue
GoogleCloudPlatform / open-match
lin-xin / vue-manage-system
```

## Getting Attributes of an element

```go
package main
import (
    "fmt"
    "log"
    "net/http"
    "github.com/PuerkitoBio/goquery"
)

func scrapeAttributes(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()

    if resp.StatusCode != 200{
        log.Fatalf("Status code error: %d %s", resp.StatusCode, resp.Status)
    }
    doc, err := goquery.NewDocumentFromReader(resp.Body)
    if err != nil{
        log.Fatal(err)
    }
    fmt.Println(doc.Find("title").Text())

    doc.Find("ol li").Each(func(i int, s *goquery.Selection){
        href, has_attr := s.Find("a").First().Attr("href")
        if has_attr{
            fmt.Println("https://github.com" + href)
        }

    })
}

func main(){
    scrapeAttribtutes()
}
```

**Output**
```
$ go run example.go
Trending repositories on GitHub today · GitHubAsset 1Asset 1
https://github.com/you-dont-need/You-Dont-Need-Momentjs
https://github.com/ripienaar/free-for-dev
https://github.com/Nozbe/WatermelonDB
https://github.com/cjbarber/ToolsOfTheTrade
https://github.com/byoungd/English-level-up-tips-for-Chinese
https://github.com/TheAlgorithms/Python
https://github.com/thedaviddias/Front-End-Checklist
https://github.com/zziz/pwc
https://github.com/dawnlabs/carbon
https://github.com/CyC2018/CS-Notes
https://github.com/Avik-Jain/100-Days-Of-ML-Code
https://github.com/donnemartin/system-design-primer
https://github.com/mariusandra/pigeon-maps
https://github.com/Snailclimb/JavaGuide
https://github.com/JavaNoober/BackgroundLibrary
https://github.com/crossoverJie/JCSprout
https://github.com/Microsoft/nni
https://github.com/PansonPanson/Java-Notes
https://github.com/date-fns/date-fns
https://github.com/sindresorhus/ky
https://github.com/mciastek/sal
https://github.com/rwv/chinese-dos-games
https://github.com/vuejs/vue
https://github.com/GoogleCloudPlatform/open-match
https://github.com/lin-xin/vue-manage-system
```

## Using class name or other attributes to get element

```go
package main
import (
    "fmt"
    "log"
    "strings"
    "net/http"
    "github.com/PuerkitoBio/goquery"
)
func scrapeViaClassName(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()

    if resp.StatusCode != 200{
        log.Fatalf("Status code error: %d %s", resp.StatusCode, resp.Status)
    }
    doc, err := goquery.NewDocumentFromReader(resp.Body)
    if err != nil{
        log.Fatal(err)
    }
    fmt.Println(doc.Find("title").Text())

    doc.Find("ol li").Each(func(i int, s *goquery.Selection){
        fmt.Println(strings.TrimSpace(s.Find(".float-sm-right").Text()))
    })
}


func main(){
    scrapeViaClassName()
}
```

**Output**
```
$ go run example.go
Trending repositories on GitHub today · GitHub
625 stars today
476 stars today
407 stars today
392 stars today
332 stars today
316 stars today
304 stars today
274 stars today
249 stars today
201 stars today
206 stars today
188 stars today
192 stars today
165 stars today
154 stars today
141 stars today
153 stars today
146 stars today
153 stars today
149 stars today
145 stars today
134 stars today
124 stars today
137 stars today
117 stars today
```

## Navigate childrens from an element

```go
package main
import (
    "fmt"
    "log"
    "strings"
    "net/http"
    "github.com/PuerkitoBio/goquery"
)
func navigateChildrens(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()

    if resp.StatusCode != 200{
        log.Fatalf("Status code error: %d %s", resp.StatusCode, resp.Status)
    }
    doc, err := goquery.NewDocumentFromReader(resp.Body)
    if err != nil{
        log.Fatal(err)
    }
    fmt.Println(doc.Find("title").Text())
    olSelection := doc.Find("ol")
    olSelection.Children().Each(func(i int, s *goquery.Selection){ // using .Children() on the ol selection to get all li
        fmt.Println(strings.TrimSpace(s.Find("h3").Text()))
    })
}


func main(){
    navigateChildrens()
}
```

**Output**
```
$ go run example.go
Trending repositories on GitHub today · GitHub
you-dont-need / You-Dont-Need-Momentjs
ripienaar / free-for-dev
Nozbe / WatermelonDB
cjbarber / ToolsOfTheTrade
byoungd / English-level-up-tips-for-Chinese
TheAlgorithms / Python
thedaviddias / Front-End-Checklist
zziz / pwc
dawnlabs / carbon
CyC2018 / CS-Notes
Avik-Jain / 100-Days-Of-ML-Code
donnemartin / system-design-primer
mariusandra / pigeon-maps
Snailclimb / JavaGuide
JavaNoober / BackgroundLibrary
crossoverJie / JCSprout
Microsoft / nni
PansonPanson / Java-Notes
date-fns / date-fns
sindresorhus / ky
mciastek / sal
rwv / chinese-dos-games
vuejs / vue
GoogleCloudPlatform / open-match
lin-xin / vue-manage-system
```

The .children will only return the immediate childrens of the parent element.

## Navigating previous and next siblings of an element

```go
package main
import (
    "fmt"
    "log"
    "strings"
   "net/http"
    "github.com/PuerkitoBio/goquery"
)

func navigateSiblings(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()
    if resp.StatusCode != 200{
        log.Fatalf("Status code error: %d %s", resp.StatusCode, resp.Status)
    }
    doc, err := goquery.NewDocumentFromReader(resp.Body)
    if err != nil{
        log.Fatal(err)
    }
    fmt.Println(doc.Find("title").Text())
    liSelection := doc.Find("ol li")
    fifthElement := liSelection.Eq(4) // using Eq() and passing the index we can navigate to the element with given index
    fmt.Println(strings.TrimSpace(fifthElement.Find("h3").Text()))
    fourthElement := fifthElement.Prev()
    fmt.Println(strings.TrimSpace(fourthElement.Find("h3").Text()))
    sixthElement := fifthElement.Next()
    fmt.Println(strings.TrimSpace(sixthElement.Find("h3").Text()))
}


func main(){
   navigateSiblings()
}
```

**Output**
```
$ go run example.go
Trending repositories on GitHub today · GitHub
byoungd / English-level-up-tips-for-Chinese
cjbarber / ToolsOfTheTrade
TheAlgorithms / Python
```

## Putting it all together(Github Trending Scraper using Golang)

```go
package main
import (
    "fmt"
    "log"
    "strings"
    //"io"
    //"os"
    "net/http"
    "github.com/PuerkitoBio/goquery"
)

func githubTrendingScraper(){
    resp, err := http.Get("https://github.com/trending")
    if err != nil{
        log.Fatal(err)
    }
    defer resp.Body.Close()
    if resp.StatusCode != 200{
        log.Fatalf("Status code error: %d %s", resp.StatusCode, resp.Status)
    }
    doc, err := goquery.NewDocumentFromReader(resp.Body)
    if err != nil{
        log.Fatal(err)
    }

    fmt.Println(doc.Find("title").Text())
    doc.Find("ol li").Each(func (i int, s *goquery.Selection){
        repositoryName := strings.TrimSpace(s.Find("h3").Text())
        totalStarsToday := strings.TrimSpace(s.Find(".float-sm-right").Text())
        href, has_attr := s.Find("a").Attr("href")
        if !has_attr{
            href = "No valid url found"
        }
        fmt.Println(repositoryName, "\t", totalStarsToday, "\t", "https://github.com" + href)
    })

}


func main(){
   githubTrendingScraper()
}
```

**Output**
```
$ go run example.go
Trending repositories on GitHub today · GitHub
you-dont-need / You-Dont-Need-Momentjs      625 stars today https://github.com/you-dont-need/You-Dont-Need-Momentjs
ripienaar / free-for-dev                    476 stars today https://github.com/ripienaar/free-for-dev
Nozbe / WatermelonDB                        407 stars today https://github.com/Nozbe/WatermelonDB
cjbarber / ToolsOfTheTrade                  392 stars today https://github.com/cjbarber/ToolsOfTheTrade
byoungd / English-level-up-tips-for-Chinese 332 stars today https://github.com/byoungd/English-level-up-tips-for-Chinese
TheAlgorithms / Python                      316 stars today https://github.com/TheAlgorithms/Python
thedaviddias / Front-End-Checklist          304 stars today https://github.com/thedaviddias/Front-End-Checklist
zziz / pwc                                  274 stars today https://github.com/zziz/pwc
dawnlabs / carbon                           249 stars today https://github.com/dawnlabs/carbon
CyC2018 / CS-Notes                          201 stars today https://github.com/CyC2018/CS-Notes
Avik-Jain / 100-Days-Of-ML-Code             206 stars today https://github.com/Avik-Jain/100-Days-Of-ML-Code
donnemartin / system-design-primer          188 stars today https://github.com/donnemartin/system-design-primer
mariusandra / pigeon-maps                   192 stars today https://github.com/mariusandra/pigeon-maps
Snailclimb / JavaGuide                      165 stars today https://github.com/Snailclimb/JavaGuide
JavaNoober / BackgroundLibrary              154 stars today https://github.com/JavaNoober/BackgroundLibrary
crossoverJie / JCSprout                     141 stars today https://github.com/crossoverJie/JCSprout
Microsoft / nni                             153 stars today https://github.com/Microsoft/nni
PansonPanson / Java-Notes                   146 stars today https://github.com/PansonPanson/Java-Notes
date-fns / date-fns                         153 stars today https://github.com/date-fns/date-fns
sindresorhus / ky                           149 stars today https://github.com/sindresorhus/ky
mciastek / sal                              145 stars today https://github.com/mciastek/sal
rwv / chinese-dos-games                     134 stars today https://github.com/rwv/chinese-dos-games
vuejs / vue                                 124 stars today https://github.com/vuejs/vue
GoogleCloudPlatform / open-match            137 stars today https://github.com/GoogleCloudPlatform/open-match
lin-xin / vue-manage-system                 117 stars today https://github.com/lin-xin/vue-manage-system
```