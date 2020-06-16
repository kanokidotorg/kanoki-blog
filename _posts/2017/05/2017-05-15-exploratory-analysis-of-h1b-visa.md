---
title: "Exploratory Analysis of H1B Visa"
date: "2017-05-15"
categories: [ Data Science, Python ]
tags: [ DataScience, Python, Matplotlib ]
---

The H-1B is a non-immigrant visa in the United States, it is designed to bring foreign professionals with college degrees and specialized skills to fill jobs when qualified Americans cannot be found. However the outsourcing companies in recent years have dominated the entire H1B visa program.

Will analyze the H1B visa data from 2011 and find out how this visa program has been used around the Globe. I will be using Python as my primary language for this analysis.

**Import all the required libraries Pandas, Matplotlib & Seaborn**

> import pandas as pd import matplotlib.pyplot as plt %matplotlib inline import seaborn as sns import plotly.plotly as py

 **Read the data and import in pandas dataframe**

> df = pd.read\_csv('./h1b\_kaggle.csv')

**Drop the null values from the list**

> df=df.dropna()

**What are the different H1B visa status in this entire dataset**

![Screen Shot 2017-05-15 at 21.58.33](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-21-58-33.png)

**Top five Employers who have filed maximum H1B visa applications from 2011-2016**

![Screen Shot 2017-05-15 at 21.58.46](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-21-58-46.png)

**No. of visa application trends from 2011-2016 for Top five Employers, We can see a growth in application filing for all these employers every year, While Infosys Limited has topped the list across all the years**

![Screen Shot 2017-05-15 at 21.58.58](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-21-58-58.png)

**In 2016, These are the top ten employers in terms of no. of applications**

![Screen Shot 2017-05-15 at 22.00.19](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-22-00-19.png)

**Look at the significant increase in the application for all the years from 2011-2016**

![Screen Shot 2017-05-15 at 22.01.24](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-22-01-24.png)

**For all the visa application filed so far, only 3-5% has been denied but rest of them are Certified**

![Screen Shot 2017-05-15 at 22.01.36](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-22-01-36.png)

**Let’s look at the top 25 Job titles employed using H1B visa, Programmer Analyst has flooded the entire job market for H1B visa followed by Software Engineer.**

![Screen Shot 2017-05-15 at 22.01.57](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-22-01-57.png)

**The top 10 cities hiring the H1B visa holders, New York topping the list**

![Screen Shot 2017-05-15 at 22.02.11](https://techpickup.files.wordpress.com/2017/05/screen-shot-2017-05-15-at-22-02-11.png)
