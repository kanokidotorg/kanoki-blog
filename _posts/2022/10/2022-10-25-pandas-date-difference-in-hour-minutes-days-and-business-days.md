---
title: "Pandas date difference in hours, minutes, days and business days"
date: 2022-10-25
categories: [pandas]
tags: [pandas]
last_modified_at: 2022-10-25
permalink: pandas-date-difference-in-hour-minutes-days-and-business-days
---

In this post we want to find the difference(Timedelta) to represent a duration, the difference between two dates or times.

We will use pandas dt, An accessor object for date time like properties of the Series value to get number of days and total seconds from the Timedelta object.

Will also see how to extract the components of Timedelta.

## <span style="color:blue">Difference between two dates </span>

Let's create a dataframe with two datetime columns, start_date and end_date.

```python
data = {'start_date': [pd.Timestamp('2022-01-24 13:03:12.050000'), pd.Timestamp('2022-01-27 11:57:18.240000'), pd.Timestamp('2014-01-23 10:07:47.660000')],
        'end_date': [pd.Timestamp('2022-02-10 23:41:21.870000'), pd.Timestamp('2022-01-31 15:38:22.540000'), pd.Timestamp('2014-01-31 18:50:41.420000')]}

df = pd.DataFrame(data)
df
```

|      | starte_date             | end_date                  |
| ---- | ----------------------- | ------------------------- |
| 0    | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 |
| 1    | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 |
| 2    | 2014-01-23 10:07:47.660 | 2014-01-31 18:50:41.42035 |

### <span style="color:brown">Difference  between two dates in days</span>

We want to get the difference between end_date and start_date columns in days

```python
# days
df['days_diff'] = (df.end_date-df.start_date).dt.days
```

|      |       starte_date       |         end_date          | days_diff |
| :--: | :---------------------: | :-----------------------: | :-------: |
|  0   | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 |    17     |
|  1   | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 |     4     |
|  2   | 2014-01-23 10:07:47.660 | 2014-01-31 18:50:41.42035 |     8     |

### <span style="color:brown">Difference  between two dates in hours</span>

Let's get the difference between end_date and start_date columns in hours

We will use total_seconds() function that returns total duration of each element expressed in seconds and divide it by 3600 to get the duration in hours

```python
# hours
df[hrs_diff] = (df.end_date-df.start_date).dt.total_seconds()/3600
```

<br>Alternatively, we can use as_type() method because Pandas timestamp differences returns a datetime.timedelta object. This can easily be converted into hours by using the *as_type* method

```python
df[hrs_diff] = (df.end_date-df.start_date).astype('timedelta64[h]')
```



|      |       starte_date       |         end_date          |  hrs_diff  |
| :--: | :---------------------: | :-----------------------: | :--------: |
|  0   | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 | 418.636061 |
|  1   | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 | 99.684528  |
|  2   | 2014-01-23 10:07:47.660 | 2014-01-31 18:50:41.42035 | 200.714933 |

### <span style="color:brown">Difference  between two dates in minutes</span>

To convert the duration in minutes we can divide the return value of total_seconds() by 60

```python
# minutes
df[mins_diff] = (df.end_date-df.start_date).dt.total_seconds()/60
```



|      |       starte_date       |         end_date          |  mins_diff   |
| :--: | :---------------------: | :-----------------------: | :----------: |
|  0   | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 | 25118.163667 |
|  1   | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 | 5981.071667  |
|  2   | 2014-01-23 10:07:47.660 | 2014-01-31 18:50:41.42035 | 12042.896000 |



### <span style="color:brown">Timedelta Date components as dataframe</span>

The dt.components returns the Dataframe of the components of the Timedeltas

```python
(df.end_date-df.start_date).dt.components
```

|      | days | hours | minutes | seconds | milliseconds | microseconds | nanoseconds |
| ---- | ---- | ----- | ------- | ------- | ------------ | ------------ | ----------- |
|      | 17   | 10    | 38      | 9       | 820          | 0            | 0           |
|      | 4    | 3     | 41      | 4       | 300          | 0            | 0           |
|      | 8    | 8     | 42      | 53      | 760          | 0            | 0           |

Here we want to get just the minutes component from the timedelta object. Similarly you can get all the columns of the above dataframe

```python
(df.end_date-df.start_date).dt.components.minutes
```

```python
0    38
1    41
2    42
Name: minutes, dtype: int64
```

### <span style="color:brown">Date difference from today</span>

We want to calculate the date difference between today's date and any date column(end_date) in the dataframe.

Using pd.to_datetime('today') return current timestamp (not just date) in local timezone, irrespective of the actual time and can be used for comparison.

Replacing `today` with `now` would give you the date in UTC instead of local time and in neither case the timezone info is added

```python
df['days_diff_from_today'] = (pd.to_datetime('today')-df.end_date).dt.days
```



|      |       starte_date       |         end_date          | **days_diff_from_today** |
| :--: | :---------------------: | :-----------------------: | :----------------------: |
|  0   | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 |           256            |
|  1   | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 |           267            |
|  2   | 2022-01-23 10:07:47.660 | 2022-01-31 18:50:41.42035 |           266            |

Similarly, using the total_seconds() function we can compute the elpased hours between the dates

```python
df['hour_diff_from_today'] = (pd.to_datetime('today')-df.end_date).dt.total_seconds()/3600
```



|      |       starte_date       |         end_date          | **hour_diff_from_today** |
| :--: | :---------------------: | :-----------------------: | :----------------------: |
|  0   | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 |       6161.288272        |
|  1   | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 |       6409.338086        |
|  2   | 2022-01-23 10:07:47.660 | 2022-01-31 18:50:41.42035 |       6406.132841        |

### <span style="color:brown">Business hours between two dates</span>

We could use the date_range() to get a fixed frequency DatetimeIndex with business day as the default.

Pandas 1.4.0, There is an inclusive parameter to include boundaries with the following options: both, neither, left, right

```python
# business date range
f = lambda x: (len(pd.bdate_range(x['todate'], x['fromdate'], inclusive='right')))
df['bday_diff']=df.apply(f, axis=1)
df
```

|      |       starte_date       |         end_date          | **bday_diff** |
| :--: | :---------------------: | :-----------------------: | :-----------: |
|  0   | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 |      13       |
|  1   | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 |       2       |
|  2   | 2022-01-23 10:07:47.660 | 2022-01-31 18:50:41.42035 |       6       |

Note, The difference in days for third row is 6 since we are including the right enddate

OR

Numpy busyday_count() function counts the number of valid days between *begindates* and *enddates*, not including the day of *enddates*

If the datetime columns are not then convert to 'datetime64[D]'` before using `np.busday_count`

```python
#business date range
A = [d.date() for d in df['fromdate']]
B = [d.date() for d in df['todate']]
df['bday_diff']=np.busday_count(B, A)
df
```

Note, The third row business day difference is 5 and not 6 since it excludes both enddates

|      |       starte_date       |         end_date          | **bday_diff** |
| :--: | :---------------------: | :-----------------------: | :-----------: |
|  0   | 2022-01-24 13:03:12.050 | 2022-02-10 23:41:21.87050 |      13       |
|  1   | 2022-01-27 11:57:18.240 | 2022-01-31 15:38:22.54042 |       2       |
|  2   | 2022-01-23 10:07:47.660 | 2022-01-31 18:50:41.42035 |       5       |

