---
title: "Data Analysis of IMDB Data"
date: "2017-05-14"
categories: [ Data Science, Python ]
---

\[youtube https://www.youtube.com/watch?v=mS3dzczv1ZQ?version=3&rel=1&fs=1&autohide=2&showsearch=0&showinfo=1&iv\_load\_policy=1&start=1&wmode=transparent\]

We all are surrounded by data and it reveals lot of things to us to make our decisions and recommends the next steps. Data is collected from different sources such as Web, Database, log files etc. and then it is thoroughly cleaned and reshaped, and further used for analysis and explored to determine the hidden patterns and trends which is really essential for any business decision making, Extracting data from web is always easy with the help of API’s but what if website doesn’t provide any API’s, In such case, Web Scraping is an excellent way to extract the unstructured data from web and put that in structured format like excel,csv, database etc..

### Web Scraping with Selenium Webdriver

- if there is any content on the page rendered by javascript then Selenium webdriver wait for the entire page to load before crwaling whereas other libs like BeautifulSoup,Scrapy and Requests works only on static pages.
- Any browser actions can be done with the help of Selenium webdriver, if there is any content on the page displayed by on button click or Scrolling or Page Navigation

### Using IPython Notebook

#### Some Imports

![](https://blogvbweb.files.wordpress.com/2017/04/5b5af-1cyhdd3cbr8fptf3hjwdtqg.png)

**Open Page in Chrome Browser**

![](https://blogvbweb.files.wordpress.com/2017/04/1385e-1x9mvmpwmqhw8xluwxnqdyw.png)

![](https://blogvbweb.files.wordpress.com/2017/04/ddeeb-14gdzjd4qrwyulvmkszpqeq.png)

#### Python Function to Scrap Data using Selenium Webdriver

Just provide the address(Xpath or any other locator) of the data to be extracted and Selenium webdriver extracts all the data from the page just with the help of one api(find_element_by_xpath), See how easy it is. Isn’t it?

![](https://blogvbweb.files.wordpress.com/2017/04/4354c-1s0zz_ppc1wt7q6vuw-ipqa.png)

Data is extracted from the page with the help of webdriver and is stored in a list, So individual list for all the following data is created:

- Movie Name
- Release Year
- IMDB Rating
- Votes
- Director
- Genre

#### How does Extracted data looks like?

Sample Data set for Movie Name, Votes and Director is displayed here, Rest of the data is also stored in individual python list

```
**_[‘Bajirao Mastani’, ‘Queen’, ‘Bhaag Milkha Bhaag’, ‘Barfi!’, ‘Zindagi Na Milegi Dobara’]_**

**_[‘17,362’, ‘39,518’, ‘39,731’, ‘52,308’, ‘41,731’]_**

**_[‘Director: Sanjay Leela Bhansali’, ‘Director: Vikas Bahl’, ‘Director: Anurag Basu’]_**
```
Don’t see any co-relation between these data, if someone have to pick the release year and director for a movie then it’s difficult to get it from these lists, So let’s put the data in a Structured and more meaningful format which will make sense for someone looking at this data. So lets bind the data in Python dictionary

#### Python Dictionary

![](https://blogvbweb.files.wordpress.com/2017/04/61954-1er0so2uwfgf4lmi7hsns1w.png)

#### Structured Data

**_{_**

“Director”: “Director: Sanjay Leela Bhansali”,

“Votes”: “17,362”,

“RunTime”: “A historical … (158 mins.)”,

“Year”: 2015,

“Genre”: “Drama”,

“Movie Name”: “Bajirao Mastani”,

“Rating”: “7.2”

}

This Data in python dictionary(Key:value pair) looks good and make more sense now, However if you look carefully the data is not in correct format for data manipulation, Votes value contains comma, Director contains unwanted text “Director:” and Ratings and Runtime are not in correct data type. Lets Clean this data to bring it in shape for performing analysis

#### Data Cleansing

![](https://blogvbweb.files.wordpress.com/2017/04/cec79-1_nqbhfbx1gcgbzvijzh_ug.png)

**_{_**

“Director”: “Sanjay Leela Bhansali”,

“Votes”: 17362,

“RunTime”: 158,

“Year”: 2015,

“Genre”: “Drama”,

“Movie Name”: “Bajirao Mastani”,

“Rating”: 7.2

}

#### Data in Pandas Dataframe

The entire movie data is stored in python dictionary but for doing further analysis this data needs to be consumed by Pandas Dataframe so that by using Pandas rich data structures and built-in function we can do some analysis on this data. Import data in Dataframe.

![](https://blogvbweb.files.wordpress.com/2017/04/0ded8-1b4koqhutj5s0hhd6a7kvrw.png)

#### Records with Missing Values

There are some missing values in this data, But Pandas provides excellent feature to handle missing and null values. So for these 3 movies RunTime data is not available on the page. so for further analysis we will replace this missing data with the mean value of the available data for RunTimeColumn

![](https://blogvbweb.files.wordpress.com/2017/04/d280e-1v88zn1i5mx46hbrbqlwo8q.png)

#### Replace Missing Values with Mean

![](https://blogvbweb.files.wordpress.com/2017/04/c9d8a-1hxd7aeesatc6jc1nagygxw.png)

### Movies with Highest Ratings

#### Top five movies since 1955

![](https://blogvbweb.files.wordpress.com/2017/04/653c2-1qfhfeeh9iqdrbkoiqsujtg.png)

#### Ratings Trending Graph

![](https://blogvbweb.files.wordpress.com/2017/04/f1d5c-1f18e1pcemwrdwhvtwj26hw.png)

#### Movies with Lowest Ratings

![](https://blogvbweb.files.wordpress.com/2017/04/2dafd-1xnp2fheef65rpy6zal02ka.png)

### Movies with Maximum Run Time

Top 10 movies

![](https://blogvbweb.files.wordpress.com/2017/04/01341-1dnpbwylgh4b6y_cbiu2uww.png)

#### RunTime Trending Graph

![](https://blogvbweb.files.wordpress.com/2017/04/04542-1wgnn1vriibsovtuls9ygma.png)

#### Average Movie RunTime

![](https://blogvbweb.files.wordpress.com/2017/04/86d2f-1rola9jm5ebodovfa6gtszg.png)

### Movies IMDB Ratings

Movies with rating Greater than 7

![](https://blogvbweb.files.wordpress.com/2017/04/e5600-1dszzqm8mhrpwlwgx9oqbtg.png)

#### Ratings Visualization using Bar Graph

![](https://blogvbweb.files.wordpress.com/2017/04/d0a55-19kfnoeyfzf8va1n1xds2ew.png)

#### Percentage distribution

![](https://blogvbweb.files.wordpress.com/2017/04/14e3f-12fzzggbu6lminskbfj7kjg.png)

### Best Movies By Genre

![](https://blogvbweb.files.wordpress.com/2017/04/b700d-1-r-h2msrd7_slbmb6nd-5a.png)

### Directors won more than Once

![](https://blogvbweb.files.wordpress.com/2017/04/5495b-19n3uokl45vy2k8yqqgt7jg.png)

### Conclusion:

### Movies most likely to be selcted for Best Picture

- Rating greater than 7
- Run time more than 2hrs
- Category Drama and Musical

### Github:

[http://min2bro.github.io/WebScraping/](http://min2bro.github.io/WebScraping/)
