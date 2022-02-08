---

title: "How to find closest date in dataframe for a given date"
date: "2022-02-09"
categories: [ python, pandas]
tags: [ python, pandas]

---

In this article we are going to see how to search for the closest date in a dataframe for a given date

we will be following the below steps to identify the closest date for a given date:

1. Create a dataframe with datetime column
2. Use *pandas.Series.searchsorted()* to Find indices where given date should be inserted to maintain order
3. Set the index as datetime for the dataframe and use *Index.get_loc()* to get the index location for given date
4. Create a dataframe with unsorted timestamp column and use a custom function to get the closest date index

Let's get started

## Create a dataframe with one of the column as datetime

Let's create a dataframe with two columns, first column(timestamp) is type datetime and second column(values) is integer

```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta

date_today = datetime.now()
days = pd.date_range(date_today, date_today + timedelta(7), periods=8, tz='Asia/kolkata')
data = [0, 1, 2, 3, 4, 5, 6, 7]

df = pd.DataFrame({'timestamp': days, 'values': data})
```

| timestamp| values|
| ---- | --- |
| 2022-02-07 22:23:20.844126+05:30|0|
| 2022-02-07 22:53:20.844126+05:30		|1|
| 2022-02-07 23:23:20.844126+05:30 | 2
| 2022-02-07 23:53:20.844126+05:30 | 3
| 2022-02-08 00:23:20.844126+05:30 | 4
| 2022-02-08 00:53:20.844126+05:30|5
| 2022-02-08 01:23:20.844126+05:30|6
| 2022-02-08 01:53:20.844126+05:30|7


## Find closest date using searchsorted function

In order to find the closest value in a series, we can use *Series.searchsorted()* function that will find the indices into a sorted Series such that, if the corresponding elements in value were inserted before the indices, the order of series would be preserved

Here we are searching for random date(dt) '**2022-02-07T23:18:06.08349**' in the column timestamp of dataframe

```python
dt='2022-02-07T23:18:06.08349'
df. timestamp.searchsorted(dt)
```
It returns index as 2, which means the given date date(dt) '**2022-02-07T23:18:06.08349**' should be inserted at this position in the dataframe and hence give us the closest value before the search date

**Out:** 2

## Find closest date using get_loc function

To use the *Index.get_loc()* function, we have to first set the timestamp column as the index of the dataframe

*get_loc()* will return the integer location for the requested date(dt) '**2022-02-07T23:18:06.08349**' and the method **nearest** use the nearest index value if no exact match is found

```python
df = df.set_index('timestamp')
df.index.get_loc(dt, method='nearest')
```
**Out:** 2

## Find closest date in a datetime column if the dates are not sorted

So far we have seen to find the index of closest date when the dates in column are sorted i.e. monotonically increasing or decreasing

In this section, we will see how to find the closest datetime index for a given date when the timestamp is not sorted

Let's create a dataframe with random non-sorted timestamp column. See [this](https://stackoverflow.com/questions/50559078/generating-random-dates-within-a-given-range-in-pandas?noredirect=1&lq=1) post in Stackoverflow

```python
def random_dates(start, end, n=8):

    start_u = start.value//10**9
    end_u = end.value//10**9

    return pd.to_datetime(np.random.randint(start_u, end_u, n), unit='s')

start = pd.to_datetime('2015-01-01')
end = pd.to_datetime('2018-01-01')

days = random_dates(start, end)
data = [0, 1, 2, 3, 4, 5, 6, 7]
df = pd.DataFrame({'timestamp': pd.to_datetime(days), 'values': data})
df
```

| timestamp| values|
| ---- | --- |
| 2015-01-10 11:17:20|0|
| 	2015-02-09 02:45:05		|1|
| 	2015-03-28 14:43:37	 | 2
| 	2017-11-19 12:37:43	 | 3
| 2015-07-19 02:40:55	 | 4
| 2016-01-26 12:43:29|5
| 	2016-09-20 19:06:39|6
|2017-05-28 13:18:12	|7


Next we will search for **date(dt) = '2016-12-07T23:18:06.08349'** in the timestamp column

First, we have to create a function to find the nearest date in a list, I've used this function from [this](https://stackoverflow.com/questions/32237862/find-the-closest-date-to-a-given-date) stackoverflow post

```python
def nearest(items, pivot):
    return pd.to_datetime(min([i for i in items if i <= pivot], key=lambda x: abs(x - pivot)))
```

Now, we will pass the timestamp column as list to this function and find the index of the closest timestamp to the given timestamp(dt)

```python
df.loc[df.timestamp == nearest(df.timestamp.to_list(),dt)].index[0]
```
The output is index 6 which is the closest before date for dt()

**Out:** 6

## Conclusion:
 - We've seen how to use *searchsorted()* to find the closest datetime index in a pandas series 
 - *Index.get_loc() * function could be used to find the closest datetime index if the index of dataframe is datetime object
 - For non-sorted timestamp column we can build a custom function to find the closest datetime index in a Series 
