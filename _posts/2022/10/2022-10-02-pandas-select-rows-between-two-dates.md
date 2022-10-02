---
title: "Pandas select rows between two dates"
date: "2022-10-02"
categories: [python, numpy]
tags: [python, numpt]
last_modified_at: 2022-10-02
permalink: pandas-select-rows-between-two-dates

---

We want to select/filter rows between two dates of a dataframe which has a date as column/index 

There are various ways in pandas to select rows between two dates, we will discuss about each of them in the following sections

- pandas.series.between()
- Boolean Indexing
- Pandas dataframe query
- Dataframe loc
- Dataframe truncate

## <span style="color:blue">How to Select rows between date?</span>

### <span style="color:brown;font-size: 95%">1. Select rows using pandas.series.between():</span>

Let's create a dataframe with date as column and values in a date range between 10-Sep-2022 thru 2-Oct-2022

```python
df=pd.DataFrame(pd.date_range('10-sep-2022','2-oct-2022',freq='9h'),columns=['timestamp'])
df.head()
```

|      | date                |
| ---- | ------------------- |
| 0    | 2022-09-10 00:00:00 |
| 1    | 2022-09-10 09:00:00 |
| 2    | 2022-09-10 18:00:00 |
| 3    | 2022-09-11 03:00:00 |
| 4    | 2022-09-11 12:00:00 |

We want to filter the rows in this dataframe(df) where rows are between 15-SEP-2022 and 2-OCT-2022

pandas.series.between() function returns a boolean vector containing True wherever the corresponding Series element is between the boundary values left and right. NA values are treated as False

**Note:** If your datetime column have the Pandas datetime type (e.g. `datetime64[ns]`), for proper filtering you need the [pd.Timestamp object](https://pandas.pydata.org/pandas-docs/version/0.22.0/timeseries.html),

```python
df[df.date.between('15-sep-2022','21-sep-2022')]
```

Out:

|      | date                |
| ---- | ------------------- |
| 14   | 2022-09-15 06:00:00 |
| 15   | 2022-09-15 15:00:00 |
| 16   | 2022-09-16 00:00:00 |
| 17   | 2022-09-16 09:00:00 |
| 18   | 2022-09-16 18:00:00 |
| 19   | 2022-09-17 03:00:00 |
| 20   | 2022-09-17 12:00:00 |
| 21   | 2022-09-17 21:00:00 |
| 22   | 2022-09-18 06:00:00 |
| 23   | 2022-09-18 15:00:00 |
| 24   | 2022-09-19 00:00:00 |
| 25   | 2022-09-19 09:00:00 |
| 26   | 2022-09-19 18:00:00 |
| 27   | 2022-09-20 03:00:00 |
| 28   | 2022-09-20 12:00:00 |
| 29   | 2022-09-20 21:00:00 |

### <span style="color:brown;font-size: 95%">2. Filter rows using Boolean Indexing:</span>

We could use boolean indexing to select the rows between the dates, you can pass the dates as `datetime.datetime`,`np.datetime64`, `pd.Timestamp`, or even `datetime strings`

```python
df[(df['date'] > '2022-09-15') & (df['date'] < '2022-09-21')]
```

Let's suppose your date column is type of datetime64[ns] then you can use dt accessor to filter dates with specific format

```python
df[(df['date'].dt.strftime('%Y-%m-%d') >= '2022-09-15')&(df['date'].dt.strftime('%Y-%m-%d') <= '2022-09-20')]
```



Out:

|      | date                |
| ---- | ------------------- |
| 14   | 2022-09-15 06:00:00 |
| 15   | 2022-09-15 15:00:00 |
| 16   | 2022-09-16 00:00:00 |
| 17   | 2022-09-16 09:00:00 |
| 18   | 2022-09-16 18:00:00 |
| 19   | 2022-09-17 03:00:00 |
| 20   | 2022-09-17 12:00:00 |
| 21   | 2022-09-17 21:00:00 |
| 22   | 2022-09-18 06:00:00 |
| 23   | 2022-09-18 15:00:00 |
| 24   | 2022-09-19 00:00:00 |
| 25   | 2022-09-19 09:00:00 |
| 26   | 2022-09-19 18:00:00 |
| 27   | 2022-09-20 03:00:00 |
| 28   | 2022-09-20 12:00:00 |
| 29   | 2022-09-20 21:00:00 |



### <span style="color:brown;font-size: 95%">3. Select rows using Dataframe query</span>

We could also use the query to select the rows between the dates

```python
df.query('20220915 < date < 20220921')
```

Out:

|      | date                |
| ---- | ------------------- |
| 14   | 2022-09-15 06:00:00 |
| 15   | 2022-09-15 15:00:00 |
| 16   | 2022-09-16 00:00:00 |
| 17   | 2022-09-16 09:00:00 |
| 18   | 2022-09-16 18:00:00 |
| 19   | 2022-09-17 03:00:00 |
| 20   | 2022-09-17 12:00:00 |
| 21   | 2022-09-17 21:00:00 |
| 22   | 2022-09-18 06:00:00 |
| 23   | 2022-09-18 15:00:00 |
| 24   | 2022-09-19 00:00:00 |
| 25   | 2022-09-19 09:00:00 |
| 26   | 2022-09-19 18:00:00 |
| 27   | 2022-09-20 03:00:00 |
| 28   | 2022-09-20 12:00:00 |
| 29   | 2022-09-20 21:00:00 |

You could also use `pd.Timestamp` to perform a query as shown below

```python
ts = pd.Timestamp
df.query('@ts("20220915T100000") <= date < @ts("20220921T120000")')
```

Out:

|      | date                |
| ---- | ------------------- |
| 15   | 2022-09-15 15:00:00 |
| 16   | 2022-09-16 00:00:00 |
| 17   | 2022-09-16 09:00:00 |
| 18   | 2022-09-16 18:00:00 |
| 19   | 2022-09-17 03:00:00 |
| 20   | 2022-09-17 12:00:00 |
| 21   | 2022-09-17 21:00:00 |
| 22   | 2022-09-18 06:00:00 |
| 23   | 2022-09-18 15:00:00 |
| 24   | 2022-09-19 00:00:00 |
| 25   | 2022-09-19 09:00:00 |
| 26   | 2022-09-19 18:00:00 |
| 27   | 2022-09-20 03:00:00 |
| 28   | 2022-09-20 12:00:00 |
| 29   | 2022-09-20 21:00:00 |
| 30   | 2022-09-21 06:00:00 |



### <span style="color:brown;font-size: 95%">4. Select rows using loc</span>

For using the .loc, we need to first set the date column as index

```python
df=df.set_index('date')
```

After that sort the index 

```python
df.sort_index(inplace=True, ascending=True)
```

and eventually select the rows between the dates

```python
df.loc['2022-09-15':'2022-09-20']
```

Out:

| date                |
| ------------------- |
| 2022-09-15 06:00:00 |
| 2022-09-15 15:00:00 |
| 2022-09-16 00:00:00 |
| 2022-09-16 09:00:00 |
| 2022-09-16 18:00:00 |
| 2022-09-17 03:00:00 |
| 2022-09-17 12:00:00 |
| 2022-09-17 21:00:00 |
| 2022-09-18 06:00:00 |
| 2022-09-18 15:00:00 |
| 2022-09-19 00:00:00 |
| 2022-09-19 09:00:00 |
| 2022-09-19 18:00:00 |
| 2022-09-20 03:00:00 |
| 2022-09-20 12:00:00 |
| 2022-09-20 21:00:00 |

### 

### <span style="color:brown;font-size: 95%">5. Select rows using dataframe truncate</span>

It truncates a Series or DataFrame before and after some index value.

This is a useful shorthand for boolean indexing based on index values above or below certain thresholds

Because the index is a DatetimeIndex containing only dates, we can specify before and after as strings. They will be coerced to Timestamps before truncation

```python
df.set_index('date').truncate(before='2022-09-15',
            after='2022-09-20')
```

Out:

| date                |
| ------------------- |
| 2022-09-15 06:00:00 |
| 2022-09-15 15:00:00 |
| 2022-09-16 00:00:00 |
| 2022-09-16 09:00:00 |
| 2022-09-16 18:00:00 |
| 2022-09-17 03:00:00 |
| 2022-09-17 12:00:00 |
| 2022-09-17 21:00:00 |
| 2022-09-18 06:00:00 |
| 2022-09-18 15:00:00 |
| 2022-09-19 00:00:00 |
| 2022-09-19 09:00:00 |
| 2022-09-19 18:00:00 |
| 2022-09-20 03:00:00 |
| 2022-09-20 12:00:00 |
| 2022-09-20 21:00:00 |

**You can read [this](https://kanoki.org/2022/07/16/pandas-filter-dates-by-month-hour-day-or-last-n-days-weeks/) post to see how  to filter dates by month, hour, day or last n days of weeks**
