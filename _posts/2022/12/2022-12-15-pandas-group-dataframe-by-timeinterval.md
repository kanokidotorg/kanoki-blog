---
title: "Pandas Group dataframe by time interval - Hour, Month, Year, Number of days and start grouping at specific time"
date: 2022-12-17
categories: [ pandas, python]
tags: [ pandas, python]
last_modified_at: 2022-12-17
permalink: pandas-group-dataframe-by-timeinterval


---

In this post we are going to see how to group a time-series dataframe by time interval such as Hour, Month, Year, Number of days and also see how to use parameters like offset to start the grouping bin at certain specific time

Here are the steps to be followed for grouping by Time intervals:

1. We will learn about pandas grouper and resample API's
2. Create a time-series dataframe
3. Group the dataframe using grouper and resample by Year, 2 Month, 15 days, and 10 Minutes
4. Use offset parameter to start the grouping bin at a specific time 

Grouper and resample are used when the time series data has to be grouped by time intervals. Grouper and resample functions are similar in nature, both achieve the same result. Though, one may be preferred over the other depending on the data at hand.

## Create Dataframe

We will see how to group time series with time intervals using Grouper() and resample() as they both could be used to group the time-series data.

Let's create our time-series dataframe with three columns - date, sales, product - with datetime column as index.

```python
# importing required libraries
import pandas as pd

# creating dataframe with date, sales and product as columns
df = pd.DataFrame({'date': 
                   pd.to_datetime(['2020-01-11 06:29:57', 
                                   '2020-02-12 06:30:01',
                                   '2020-03-14 08:37:22', 
                                   '2020-04-16 23:11:13',
                                   '2021-05-18 23:21:43', 
                                   '2021-06-20 23:22:36',
                                   '2022-07-26 08:08:15', 
                                   '2022-08-16 16:08:56',
                                   '2022-09-04 19:05:30', 
                                   '2022-10-10 22:48:15']),
                   'sales': [955, 889, 364, 856, 754, 
                             328, 999, 652, 742, 856],
                   'product': ['Chips', 'Chocolate', 
                               'Popcorn', 'Nuts', 
                               'Crackers', 'Cookies', 
                               'Brownies', 'Marshmallows',
                               'Gummy bears', 'Jelly beans']})

# setting date as index
df = df.set_index('date')

# printing the dataframe
df
```

**Out:**

|                     | sales |   product    |
| :-----------------: | :---: | :----------: |
|        date         |       |              |
| 2020-01-11 06:29:57 |  955  |    Chips     |
| 2020-02-12 06:30:01 |  889  |  Chocolate   |
| 2020-03-14 08:37:22 |  364  |   Popcorn    |
| 2020-04-16 23:11:13 |  856  |     Nuts     |
| 2021-05-18 23:21:43 |  754  |   Crackers   |
| 2021-06-20 23:22:36 |  328  |   Cookies    |
| 2022-07-26 08:08:15 |  999  |   Brownies   |
| 2022-08-16 16:08:56 |  652  | Marshmallows |
| 2022-09-04 19:05:30 |  742  | Gummy bears  |
| 2022-10-10 22:48:15 |  856  | Jelly beans  |

## Group by Year, Month, Hour and Minutes

We want to group the above dataframe by year, month, number of days, 5hours and 10 minutes. we will use freq and rule parameters from grouper and resample respectively.

*freq:* str / frequency object, defaults to None<br>
*rule:* DateOffset, Timedelta or str<br>

`Grouper(freq = S )` and `resample(rule =)` is an argument set to a time period. This is the most important argument to group time interval data. Check out all the available [pandas frequencies here](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#offset-aliases). 

**a) Group by Year**

```python
# Grouper
df.groupby(pd.Grouper(freq = 'Y')).mean()

#OR

# resample
df.resample(rule = 'Y').mean()
```

We will use the mean() calculation throughout this article. You can use other statistical computations depending on your need. 

**Out:**

|            | sales  |
| :--------: | :----: |
|    date    |        |
| 2020-12-31 | 766.00 |
| 2021-12-31 | 541.00 |
| 2022-12-31 | 812.25 |

**b) Group by 2 months**

```python
df.groupby(pd.Grouper(freq = '2M')).sum()
```

**Out:**

|            | sales |
| :--------: | :---: |
|    date    |       |
| 2020-01-31 |  955  |
| 2020-03-31 |  626  |
| 2020-05-31 |  856  |
| 2020-07-31 |  NaN  |
| 2020-09-30 |  NaN  |

**b) Group by 15 days**

```python
df.groupby(pd.Grouper(freq = '15D')).mean()
```

**Out:**

|            | sales |
| :--------: | :---: |
|    date    |       |
| 2020-01-11 |  955  |
| 2020-01-26 |  NaN  |
| 2020-02-10 |  889  |
| 2020-02-25 |  NaN  |
| 2020-03-11 |  364  |

**b) Group by 10 minutes**

```python
df.groupby(pd.Grouper(freq = '10min')).mean()
```

**Out:**

|                     | sales |
| :-----------------: | :---: |
|        date         |       |
| 2020-01-11 06:20:00 |  955  |
| 2020-01-11 06:30:00 |  NaN  |
| 2020-01-11 06:40:00 |  NaN  |
| 2020-01-11 06:50:00 |  NaN  |
| 2020-01-11 07:00:00 |  NaN  |

## **Group by time interval and start the grouping at specific time**

We want to group the daraframe by 1 hours and 30 minutes and start the grouping from 05:45, There is an offset parameter that let you add an offset value to the origin, It takes either Timedelta or str and default is None<br>

```python
# Grouper
df.groupby(pd.Grouper(freq = '1H30min',offset = '-15min')).mean()

#OR

# resample
df.resample(rule = '2H30T', offset = '-15min').mean().head()
```

**Out:**

We have set the offset to negative 15 Minutes so the time-series starts at 5:45 and group by 1 hours and 30 minutes

|                     | sales |
| :-----------------: | :---: |
|        date         |       |
| 2020-01-11 05:45:00 | 955.0 |
| 2020-01-11 07:15:00 |  NaN  |
| 2020-01-11 08:45:00 |  NaN  |
| 2020-01-11 10:15:00 |  NaN  |
| 2020-01-11 11:45:00 |  NaN  |
