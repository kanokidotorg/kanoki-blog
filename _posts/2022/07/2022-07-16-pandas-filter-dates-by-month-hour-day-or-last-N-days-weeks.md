---
title: "Pandas filter dates by month, hour, day and last N days & weeks"
date: "2022-07-16"
categories: [ pandas, python]
tags: [ pandas, python]

---

We have dataframe with dates or timestamps columns and we would like to filter the rows by Month, Hour, day or by last n days from today's date. 

Pandas has a dt accessor object for datetimelike properties of the series and can be used to access the properties from Timestamp or a collection of timestamps like a DatetimeIndex.

if you need a refresher on working with datetime in pandas then check [this](https://kanoki.org/2019/10/09/working-with-pandas-datetime/) out

Here are some of these datetime Index properties that we will check out in this post:

* week
* weekday
* day_name
* is_month_end
* is_month_start
* is_year_end
* is_year_start

Let's get started, we will first build a dataframe with date as an index

## Create Dataframe with DatetimeIndex

We have first created a series of date range by using pd.date_range() that returns a fixed frequency DatetimeIndex and gives the range of equally spaced time points.

The start date is 1-JAN-2020 and end date is 24-DEC-2020 13:27:00, with a period of 37 min, so the total length of date series was 13955, you can expect a dataframe of these many rows.

The second column in dataframe is value and that has same length as the date column.

We have set up the date column as the Datetimeindex of the dataframe, which is Immutable ndarray-like of datetime64 data.


```python
import pandas as pd
import numpy as np

dates = pd.date_range(start='01-JAN-2020', end='24-DEC-2020 13:27:00', freq='37 min')

df=pd.DataFrame({'date':dates, 'value':np.random.uniform(-1,0,len(dates))})
df.set_index('date', inplace=True)
df.sample(2)
```
This is how our dataframe looks like

|date |value|
|-----|--------|
|2020-08-20 08:31:00	|46.53       |
|2020-08-15 07:02:00  |81.78      |
|2020-07-04 22:12:00 |84.84      |
|2020-01-29 23:36:00 |31.49      |
|2020-01-19 08:55:00 |35.61      |

## Filter by time - Hour and Minutes
We want to filter the above dataframe by a hour and minute condition, where hour is greater than or equal to 10 and less than equal to 14 and minute is greater than equal to 10 and less than equal to 30

We will use the DatetimeIndex property hour and minute to get the hour and minutes respectively from the date value or timestamp.

Since the index is a DatetimeIndex we were using *df.index.month*, however if your date is a datetime column series then you can use the dt accessor *df.date.dt.month* to get the month

```python
df[((df.index.hour>=10) & (df.index.hour<=14)) & 
   (df.index.minute>10) & (df.index.minute<30)].sort_index(ascending=True).sample(5)
```
After the rows are filter by hour and minutes condition, I'm sorting it in ascending order and grabbing a sample set of 5.

|date |value|
|-----|--------|
|2020-11-02 13:27:00	|86.03      |
|2020-11-06 14:16:00  |52.47      |
|2020-07-04 12:20:00 |87.32      |
|2020-09-27 12:16:00 |83.01     |
|2020-11-09 13:11:00 |91.82     |


## Filter by Month
Here we want to filter by month and interested to find the dates where month integer value is in 2, 7,6 and 11

We will again use the DatetimeIndex month properties to get the month from the date 

```python
df[df.index.month.isin([7, 6, 11, 2])].sort_index(ascending=True).sample(5)
```

|date |value|
|-----|--------|
|2020-07-16 01:13:00	|79.10     |
|2020-07-19 06:55:00  |72.70      |
|2020-07-30 17:20:00 |87.90      |
|2020-06-09 04:18:00|87.91     |
|2020-11-07 15:33:00 |14.82     |

We want to filter the rows from dataframe by month but this time we will use the month names and not the integer values

```python
df[df.index.month_name().isin(['July', 'June', 'November', 'February'])].sort_index(ascending=True).sample(5)
```

|date |value|
|-----|--------|
|2020-07-08 13:09:00		|79.71     |
|2020-02-07 17:16:00	 |49.45     |
|2020-02-15 15:12:00 |10.02      |
|2020-07-10 05:14:00|64.78     |
|2020-02-07 04:19:00 |72.80     |

We can also filter it by abbreviated month name by taking the slice, first 3 letters of each month   

```python
df[df.index.month_name().str[:3].isin(['Jul', 'Jun', 'Nov', 'Feb'])].sort_index(ascending=True).sample(5)
```
## Filter last 3 months

We can also filter by last N months, Since there is no month argument for pd.Timedelta so the work around is to get the month of last date and subtract N to it.

```python
df[df.index.month >= (df.index.date[-1].month-3)]
```





|date |value|
|-----|--------|
|2020-09-01 00:29:00		|94.28     |
|2020-09-01 01:06:00	 |67.04     |
|2020-09-01 01:43:00 |54.11      |
|2020-09-01 02:20:00|96.92     |
|2020-09-01 02:57:00 |97.31     |
|.... |.... |
|2020-12-24 10:30:00 |35.17 |
|2020-12-24 11:07:00 |82.86 |
|2020-12-24 11:44:00 |61.94 |
|2020-12-24 12:21:00 |49.61 |
|2020-12-24 12:58:00 |85.49 |


Alternatively, we can use pd.offsets() also but that throws a warning that comparison of Timestamp with datetime.date is deprecated and use pd.Timestamp(date) or ts.date() == date instead

```python
df[df.index.date > (df.index.date[-1]- pd.offsets.MonthBegin(3))]
```

## Filter by day of week

We want to filter the dates by day of week and the attributes that can be used for this is day_name and weekday

Here we will filter the dates by weekdays: Monday, Wednesday and Sunday from the dataframe

```python
df[df.index.day_name().isin(['Monday','Wednesday','Sunday'])].sort_index(ascending=True).head(5)
```

|date |value|
|-----|--------|
|2020-01-01 00:00:00	|12.60     |
|2020-01-01 00:37:00	 |27.79     |
|2020-01-01 01:14:00|52.52      |
|2020-01-01 01:51:00|67.40     |
|2020-01-01 02:28:00 |95.25     |

We can also filter it by abbreviated weekday name by taking the slice, first 3 letters of each day

```python
df[df.index.day_name().str[:3].isin(['Mon','Wed','Sun'])].sort_index(ascending=True).head(5)
```

Altenatively, you can also filter by index of the weekday and it starts from Monday i.e. 0, so for Mon, Wed and Sun it would be 0,2,4.

```python
df[df.index.weekday.isin([0,2,4])].sort_index(ascending=True).head(5)
```

## Filter by Month start and end

Filter by month start and end can be done by using the properties is_month_start and is_month_end

we want all the dates in dataframe which are either start of the month or end of the month

```python
df[(df.index.is_month_end)|(df.index.is_month_start)].sort_index(ascending=True).tail()
```




|date |value|
|-----|--------|
|2020-04-30 14:38:00	|37.32     |
|2020-10-01 03:13:00	 |69.76     |
|2020-09-30 03:47:00|48.22      |
|2020-05-01 00:30:00|83.94     |
|2020-09-01 10:58:00 |97.83     |


## Filter by Quarter start and end

Similar to month start and end we can use property is_quarter_start and is_quarter_end 

So we want all the dates in dataframe which are either start of a Quarter or end of a Quarter



|date |value|
|-----|--------|
|2020-10-01 23:34:00	|96.62     |
|2020-10-01 22:57:00	 |64.27     |
|2020-10-01 22:20:00 |56.88      |
|2020-10-01 21:43:00|57.	75     |
|2020-10-01 21:06:00|14.60     |
|....|.... |
|2020-01-01 02:28:00|95.25 |
|2020-01-01 01:51:00|67.40 |
|2020-01-01 01:14:00|52.52 |
|2020-01-01 00:37:00|27.79 |
|2020-01-01 00:00:00|12.60 |


## Filter last N days, Weeks, hours, seconds


We want to filter by last N days, weeks, hours and seconds, The pd.TimeDelta() represents a duration, the difference between two dates or times.

So for last N days, we could get the date at the bottom of index, which is technically the last date in dataframe and then subtract the N days timedelta from it. 

Here we are subtracting 200 days and this will give us the last 200 days rows in the dataframe

```python
df[df.index.date > (df.index.date[-1]- pd.Timedelta(days=200))]
```


|date |value|
|-----|--------|
|2020-06-08 00:33:00	|24.88     |
|2020-06-08 01:10:00	 |31.50     |
|2020-06-08 01:47:00|46.71      |
|2020-06-08 02:24:00|50.60     |
|2020-06-08 03:01:00|30.39     |
|....|.... |
|020-12-24 10:30:00|35.17 |
|2020-12-24 11:07:00|82.86 |
|2020-12-24 11:44:00|61.94 |
|2020-12-24 12:21:00|49.61 |
|2020-12-24 12:58:00|85.49 |


Similarly, to filter rows by last N weeks, hours and seconds we can just construct a Timedelta from the passed arguments, Following are the allowed keywords:

days, seconds, microseconds, milliseconds, minutes, hours, weeks

**Filter last 8 weeks:**

We want to filter last 8 weeks, so we construct a timedelta and subtracted it from the last date in dataframe

```python
df[df.index.date > (df.index.date[-1]- pd.Timedelta(weeks=8))]
```