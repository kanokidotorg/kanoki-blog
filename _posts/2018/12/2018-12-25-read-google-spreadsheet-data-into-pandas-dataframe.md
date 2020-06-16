---
title: "Read Google Spreadsheet data into Pandas Dataframe"
date: "2018-12-25"
categories: [ Data Science, google sheet, Pandas, Python ]
tags: [ GoogleSheets ]
---

**M**any a times it happens that we have our data stored on a Google drive and to analyze that data we have to export the data as csv or xlsx and store it on a disk to convert into a dataframe.

To over come this problem of Exporting and loading the data into Pandas Dataframe, I am going to show how you can directly read the data from a Google Sheet into a Pandas Dataframe.

For this Exercise I am going to use the UCI Wine Data Set:
**source:**[https://archive.ics.uci.edu/ml/datasets/wine](https://archive.ics.uci.edu/ml/datasets/wine)

import pandas as pd

Ensure that the Spreadsheet containing the data is opened in a GoogleSheet:

![](/images/2018/12/image-4.png)

Copy the URL from the Address Bar:

**google\_sheet\_url** \= 'https://docs.google.com/spreadsheets/d/19nK-I3FIgLLCK9XHSKNOPYknuq4b8-qnAuAyKUegoNQ/edit#gid=280140380'

Replace "**edit#gid**" text in the google\_sheet\_url variable above with "**export?format=csv&gid**" so your new google\_sheet\_url should look like this

![](/images/2018/12/image-5.png)

**new\_google\_sheet\_url** \= 'https://docs.google.com/spreadsheets/d/19nK-I3FIgLLCK9XHSKNOPYknuq4b8-qnAuAyKUegoNQ/_export?format=csv&gid_\=280140380'

**import pandas as pd**

Use Pandas read\_csv function to read the WineQuality Data Spreadsheet:

**df=pd.read\_csv(new\_google\_sheet\_url)**

Voila!! the data is converted into a Dataframe without downloading the csv file

**df.head()**

![](/images/2018/12/address_bar-1.jpg)
