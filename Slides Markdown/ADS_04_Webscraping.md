Applied Data Science
========================================================
author: Webscraping with R
date: 18.03.2019
autosize: false
width: 1920
height: 1080
font-family: 'Arial'
css: mySlideTemplate.css


<footer class = 'footnote'>
<div style="position: absolute; left: 0px; bottom: 50px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; left: 1100px; bottom: 25px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="320">
</div>
</footer>


Sources and Extra Material
=======

https://blog.rsquaredacademy.com/web-scraping/

https://nceas.github.io/oss-lessons/data-liberation/intro-webscraping.html

https://stanford.edu/~vbauer/files/teaching/VAMScrapingSlides.html

http://bradleyboehmke.github.io/2015/12/scraping-html-text.html

<footer class = 'footnote'>
<div style="position: absolute; left: 0px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>


Web Scraping Overview
=======

* Web scraping is an umbrella term for automated extraction of information from websites

* In turn the entirety of WWW resources are turned into a source of potential data for many different projects in business and research


* We want to discuss how to get data from the World Wide Web using R

* After a few fundamental concepts we will spent most of the time on getting data from websites that do not offer any interface to automate information retrieval


***

*Key reasons for webscraping*

* Data Format: There is a wealth of data on websites but designed for human consumption. As such, we cannot use it for data analysis as it is not in a suitable format/shape/structure.
* No copy/paste: We cannot copy & paste the data into a local file. Even if we do it, it will not be in the required format for data analysis.
* No save/download: There are no options to save/download the required data from the websites. We cannot right click and save or click on a download button to extract the required data.
* Automation: With web scraping, we can automate the process of data extraction/harvesting.

<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>


Available R Packages
=======

Relevant CRAN Task View: https://CRAN.R-project.org/view=WebTechnologies

*Major packages*

* `Rcurl` provides functions to fetch URIs, get & post forms
* `httr` interface for executing HTTP methods with support for modern web authentication protocols 
* `rvest` a higher level package which is simpler to use for basic tasks.
* `Rselenium` can be used to automate interactions and extract page content from dynamically generated webpages (i.e., those requiring user interaction to display results like clicking on button)

We will focus on `rvest`.

<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>


Basics of Web Technology (1)
=========


* Uniform Resource Locators (URL) establish request messages which facilitate basic web communication. Build up from different components:
    * Protocol (`http` or `https`)
    * Domain (`www.xyz.dom`)
    * Port (`:1234`)
    * Path (`/here/be/data/`)
    * Request(`?value=xyz`)

* A domain controls automated extraction permissions by means of the file `robots.txt` in any path
    * We can check by means of the `robotstxt::paths_allowed()`

```r
library(robotstxt)
paths_allowed(
  paths = c("http://www.transfermarkt.de/","http://www.imdb.com/title/")
)
```

```
[1] TRUE TRUE
```
    

<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>

Basics of Web Technology (2)
=========

To scrape website data some context of Web Technology and web site structures is helpful

A web page typically is made up of the following:

* HTML (Hyper Text Markup Language) takes care of the content. You need to have a basic knowledge of HTML tags as the content is typically located with these tags.
* CSS (Cascading Style Sheets) takes care of the appearance of the content. While you do not need to look into the CSS of a web page, you should be able to identify the id or class that manage the appearance of content.
* JS (Javascript) takes care of the behavior of the web page. Sites making heavy use of JS will require more careful scraping strategies.

<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>

HTML and XML
=========

* HyperText Markup Language (HTML) describes and defines the content of a webpage

    * "Hyper Text" in HTML refers to links that connect webpages to one another, either within a single website or between websites

* HTML uses "markup" to annotate text, images, and other content for display in a Web browser. HTML markup includes special "elements"" such as `<head>`, `<title>`, `<body>`, `<header>`, `<footer>`, `<article>`, `<section>`,  `<p>`, `<div>`, `<span>`, `<img>`, and many others.

* Using you web browser, you can inspect the HTML content of any webpage of the World Wide Web.

***

* The eXtensible Markup Language (XML) provides a general approach for representing all types of information, such as data sets containing numerical and categorical variables
* XML provides the basic, common, and quite simple structure and syntax for all dialects or vocabularies
    * HTML, SVG and EML are specific vocabularies of XML.



<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>

HTML Elements (1)
========

* HTML elements consist of a start tag and end tag with content inserted in between
* They can be nested and are case insensitive
* The tags can have attributes which usually come as name/value pairs
* When scraping web data we can use a combination of HTML tags and attributes to locate the content we want to extract



***

![Image](figures/htmltag.png)





<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>

HTML Elements (2)
======
Below is a list of basic HTML which are helpful for web scraping

Tag | Description
--- | ---
`<html> </html>` | root node of html document
`<head> </head>` | head node of html document
`<title> </title>` | page title
`<body> </body>` | the body stores most the main page content
`<li> </li>` | list item
`<b> </b>` | bold font
`<p> </p>` | paragaph
`<img src="xxx"> </img>` | inserts image from source `xxx`
`<a href="xxx"> </a>` | links to destintation `xxx`

***

* The Document Object Model (DOM) defines the logical structure of a document and the way it is accessed and manipulated
* HTML is structured as a tree and you should be able to characterize the path to any node or tag



![Image](figures/dom_model.png)




<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>


Scraping with `rvest`: Main functions and basic workflow
======
left: 35%

* Obtain an html document from a url, a file on disk or a string containing html with `read_html()`
* Select parts of a document using css selectors: `html_nodes(doc, "table td")` 
* Extract components with `html_tag()` (the name of the tag), `html_text()` (all text inside the tag), `html_attr()` (contents of a single attribute) and `html_attrs()` (all attributes) or parse tables into data frames with `html_table()`
* These functions can again be nicely stringed together using the pipe operator `%>%` - therefore we import both `rvest`and `tidyverse`


```r
library(tidyverse)
library(rvest)
```



***


```r
cities = "https://en.wikipedia.org/wiki/List_of_largest_cities"
paths_allowed(c(cities))
```

```
[1] TRUE
```

```r
cities %>%
  read_html() %>%
  html_table(fill = TRUE, header = TRUE) %>%
  .[[2]] %>%
  .[, c(1,2,5)] %>%
  head(10)
```

```
        City     Nation                  Population
1                                 Metropolitan area
2   Shanghai      China               24,750,000[9]
3    Beijing      China              24,900,000[11]
4      Lagos    Nigeria              21,000,000[14]
5      Dhaka Bangladesh              20,000,000[16]
6     Mumbai      India                  12,771,200
7    Chengdu      China 10,376,000[citation needed]
8    Karachi   Pakistan                            
9  Guangzhou      China              25,000,000[26]
10  Istanbul     Turkey                  13,520,000
```









<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>



Selecting Nodes
=====
* If the data is not explicitly organized as a table, we need to use formatting queues to isolate data points

* `html_node()` and `html_node()` allow us to specify css selectors or xpaths to select content on a website

* Using http://selectorgadget.com/ this can be a very easy task, sometimes we need to try around a little

***


```r
toniErdman = "http://www.imdb.com/title/tt4048272/?ref_=nv_sr_1"

toniErdman %>% 
  read_html() %>%
  html_nodes(".primary_photo+ td") %>%
  html_text()
```

```
 [1] "\n Sandra H<U+00FC>ller\n          "  
 [2] "\n Peter Simonischek\n          "     
 [3] "\n Michael Wittenborn\n          "    
 [4] "\n Thomas Loibl\n          "          
 [5] "\n Trystan P<U+00FC>tter\n          " 
 [6] "\n Ingrid Bisu\n          "           
 [7] "\n Hadewych Minis\n          "        
 [8] "\n Lucy Russell\n          "          
 [9] "\n Victoria Cocias\n          "       
[10] "\n Alexandru Papadopol\n          "   
[11] "\n Victoria Malektorovych\n          "
[12] "\n Ingrid Burkhard\n          "       
[13] "\n J<U+00FC>rg L<U+00F6>w\n          "
[14] "\n Ruth Reinecke\n          "         
[15] "\n Nicolas Wackerbarth\n          "   
```



<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>

Excursus: Cleaning strings with regular expressions
======
left: 35%


* Our results look promising so far
* However, we have already encountered artifacts that we want to get rid off
    * The population values had commas and source annotations
    * Cast list included strange non-printable artifacts
* Regular expressions (regex) are extremely useful in extracting information from any text by searching for one or more matches of a specific search pattern
* In `R` we can easily leverage the `str_replace()`, `str_replace_all()`, `str_remove()` and `str_remove_all()` funcions from `stringr` to leverage regex

https://github.com/NikoStein/ADS19/raw/master/Cheatsheets/RegexTutorialMedium.pdf

***


```r
toniErdman %>% 
  read_html() %>%
  html_nodes(".primary_photo+ td") %>%
  html_text() %>%
  .[1] -> sample

sample
```

```
[1] "\n Sandra H<U+00FC>ller\n          "
```

```r
sample %>%
  str_remove_all("\\n") %>% #\n
  str_remove_all("[ \t]+$") %>% #trailing whitespace
  str_remove_all("^[ \t]") #leading whitespace
```

```
[1] "Sandra H<U+00FC>ller"
```



<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>

Excursus: Cleaning strings with regular expressions (2)
======
left: 35%


* Our results look promising so far
* However, we have already encountered artifacts that we want to get rid off
    * The population values had commas and source annotations
    * Cast list included strange non-printable artifacts
* Regular expressions (regex) are extremely useful in extracting information from any text by searching for one or more matches of a specific search pattern
* In `R` we can easily leverage the `str_replace()`, `str_replace_all()`, `str_remove()` and `str_remove_all()` funcions from `stringr` to leverage regex

https://github.com/NikoStein/ADS19/raw/master/Cheatsheets/RegexTutorialMedium.pdf

***


```r
cities %>%
  read_html() %>%
  html_table(fill = TRUE, header = TRUE) %>%
  .[[2]] %>%
  .[3, c(1,2,4)] -> sample

sample
```

```
     City Nation     Population
3 Beijing  China 21,516,000[10]
```

```r
sample[3] %>%
  str_remove_all(",") %>% # remove commas
  str_remove_all("\\[[\\d]*\\]") %>% # remove arbitrary number of digits enclosed by square brackets 
as.numeric() #turn into number
```

```
[1] 21516000
```



<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>


Programming Task
=====

* Jeopardy! is a famous trivia show which has been running in the US for over thirty years
* All past episodes have been archived at http://www.j-archive.com/ and can be retrieved for practice or analysis
* Retrieve all questions for an episode of your choice
* Create a function that takes one argument - a string URL for the episode and return the final Jeopardy question

<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>

Subpage Navigation
=====
left: 35%


* Very often we have an overview site and then want to crawl all children sites

* Three step approach:
    * Retrieve the list of sub URLs using the techniques described before
    * Write a function which given an URL retrieves the relevant data from the subpage
    * Use `map` to execute the retrieval function on each of the URLs extracted in step 1
    
***
    

```r
overview = "http://www.j-archive.com/showseason.php?season=35"
overview %>%
  read_html() %>%
  html_nodes("td:nth-child(1) a") %>%
  html_attr("href") -> allURLs
head(allURLs,3)
```

```
[1] "http://www.j-archive.com/showgame.php?game_id=6300"
[2] "http://www.j-archive.com/showgame.php?game_id=6297"
[3] "http://www.j-archive.com/showgame.php?game_id=6295"
```

```r
getTitleOfEpisode = function(url){
  url %>% 
    read_html() %>% 
    html_node("h1") %>%
    html_text()}
map_chr(head(allURLs), getTitleOfEpisode)
```

```
[1] "Show #8003 - Wednesday, May 29, 2019"
[2] "Show #8002 - Tuesday, May 28, 2019"  
[3] "Show #8001 - Monday, May 27, 2019"   
[4] "Show #8000 - Friday, May 24, 2019"   
[5] "Show #7999 - Thursday, May 23, 2019" 
[6] "Show #7998 - Wednesday, May 22, 2019"
```
    

    
<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>    
    
Programming Task
=====

* Create an rvest script that retrieves all players from a transfermarkt.de team website
e.g., http://www.transfermarkt.de/fc-bayern-munchen/startseite/verein/27/saison_id/2018 

* Transform the script to a function that takes the entry point as a string argument

* Retrieve all the urls from the overview site:
http://www.transfermarkt.de/1-bundesliga/startseite/wettbewerb/L1/saison_id/2018 

* Use a `map` function to retrieve all players from all clubs

* Extra effort: create a function to retrieve a player's relevant details (you may want to focus on age, games played, market value and primary position) and include it accordingly in your workflow 

<footer class = 'footnote'>
<div style="position: absolute; right: 250px; bottom: 0px; z-index:100; background-color:white">
Prof. Dr. Christoph Flath <br>ADS 2019</div>
</footer>
<footer class = 'logo'>
<div style="position: absolute; right: 0px; bottom: 0px; z-index:100; background-color:white">
<img src = "uni-wuerzburg-logo.svg" width="160">
</div>
</footer>
