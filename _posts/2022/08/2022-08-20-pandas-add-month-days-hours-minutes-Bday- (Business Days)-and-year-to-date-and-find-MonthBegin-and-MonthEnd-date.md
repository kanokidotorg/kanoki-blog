---
title: "Pandas Add month, days, hours, minutes, Bday (business days) and year to date and also find out MonthBegin and MonthEnd dates"
date: "2022-08-22"
categories: [ pandas, python]
tags: [ pandas, python]

---

We want to add Month, Days, Hours, Minutes or Year offset to a date column in a dataframe and also like to see how to find out the MonthBegin and MonthEnd date for a date column.

There are two ways to add these numbers(days, months, years, hours) to a date and get the resulting date

1. DateOffset: It's a standard kind of date increment used for a date range, it can be created to move dates forward a given number of valid dates
2. Timedelta: It represents a duration, the difference between two dates or times, expressed in difference units, e.g. days, hours, minutes, seconds. They can be both positive and negative. Timedelta is the pandas equivalent of python’s datetime.timedelta 

Let's get started, we will first create a test dataframes(df) to work upon

## Create a Dataframe


This dataframe has a date column and just two rows so it would be easier to analyze and verify the results


```python
import pandas as pd
import numpy as np

df = pd.DataFrame([pd.Timestamp('20220831'),
                   pd.Timestamp('20220824') ], columns=['date'])
df
```


|Date|
|-----|
|2022-08-31	|
|2022-08-24 |


## Add month to date

we want to add 3 months to the date column in dataframe, so we can use DataOffset to increment or decrement the date and it has month parameter to add or replace the offset value

```python
df['Date_after_3month'] = df.date + pd.DateOffset(months=3)
```

The Date_after_3month column displays the date when 3 months are added to date column

|Date| Date\_after_3month|
|:-----:|:-----:|
|2022-08-31	|2022-11-30|
|2022-08-24 |2022-11-24|



## Add hours to date

We could use either DateOffset or Timedelta to add hours to date, Pandas timedelta has unit parameter that have following possible string value to denote the type of date i.e. days, hours, minutes, seconds, milliseconds, microseconds, nanoseconds and ‘W’, ‘D’, ‘T’, ‘S’, ‘L’, ‘U’, or ‘N’

In this case, we want to add 3 hours to the date column in dataframe

```python
df['date_after_3hrs'] = df.date + DateOffset(hours=3)
df
```

Or

```python
df['date_after_3hrs'] = df.date + pd.Timedelta(3,unit='h')
df
```



The new column date_after_3hrs displays the date and time when 3 hours are added to the date column

|    Date    |   date_after_3hrs   |
| :--------: | :-----------------: |
| 2022-08-31 | 2022-08-31 03:00:00 |
| 2022-08-24 | 2022-08-24 03:00:00 |


## Add days to date

We want to add 15 days to the date column and we can use either pandas DateOffset or Timedelta to do this.

```python
df['date_after_15days'] = df.date+pd.Timedelta(15,unit='d')
df
```

Or

```python
df['date_after_15days'] = df.date + DateOffset(days=15)
df
```

The new column date_after_15days shows the 15 days ahead of date values in the date column for each row 

|    Date    | date_after_15days |
| :--------: | :---------------: |
| 2022-08-31 |    2022-09-15     |
| 2022-08-24 |    2022-09-08     |

## Add Weeks to date

We could also add Weeks by using the weeks parameter in DateOffset and unit value 'W' in timedelta.

```python
df['date_after_1week'] = df.date+pd.Timedelta(1,unit='W')
```

Or

```python
df['date_after_1week'] = df.date+DateOffset(weeks=1)
df
```

The new column date_after_1week shows a week ahead date 

|    Date    | date_after_1week |
| :--------: | :--------------: |
| 2022-08-31 |    2022-09-07    |
| 2022-08-24 |    2022-08-31    |

## Add bdays (Business days) to date

If you want to add the business days and not weekends to a date then Bday defines this set to be the set of dates that are weekdays (M-F). For example, Bday(5) can be added to a date to move it five business days forward

```python
from pandas.tseries.offsets import BDay
df['date_after_5bday'] = df.date+BDay(5)
df
```

The column date_after_5bday shows the 5 business day ahead of date column

|    Date    | **date_after_5bday** |
| :--------: | :------------------: |
| 2022-08-31 |      2022-09-07      |
| 2022-08-24 |      2022-08-31      |

## Add Years to date

We can also add Year to a date using the years parameter of DateOffset. However we cannot use pandas timedelta because it doesn't support Month, Year values in unit parameters 

```python
df['date_after_2years'] = df.date + DateOffset(years=2)
```

The date_after_2years shows the date 2 years ahead of the date column

|    Date    | **date_after_2years** |
| :--------: | :-------------------: |
| 2022-08-31 |      2024-08-31       |
| 2022-08-24 |      2024-08-24       |

## Month End Date

We can use class ***pandas.tseries.offsets.****MonthEnd(n)* for DateOffset of one month end.

For the case when `n=0`, the date is not moved if on an anchor point, otherwise it is rolled forward to the next anchor point.

The first date value is 2022-08-31 which is an anchor point, so passing  n=0 will not move it, whereas the second date value 2022-08-24 moves it to the next anchor point i.e. 2022-08-31

```python
from pandas.tseries.offsets import MonthEnd

df['month_end_date'] = df['date'] + MonthEnd(0)
df
```

|    Date    | **month_end_date** |
| :--------: | :----------------: |
| 2022-08-31 |     2022-08-31     |
| 2022-08-24 |     2022-08-31     |

## Next Month End Date

Here we have passed n=`1` in `MonthEnd` just specifies to move one step forward to the next date that's a month end. If you wanted the last day of the next two months, you'd use `MonthEnd(2)`, etc. This should work for any month, so you don't need to know the number days in the month

```python
from pandas.tseries.offsets import MonthEnd

df['next_month_end_date'] = df['date'] + MonthEnd(1)
df
```

|    Date    | **next_month_end_date** |
| :--------: | :---------------------: |
| 2022-08-31 |       2022-09-30        |
| 2022-08-24 |       2022-08-31        |

## Month Begin Date

Similarly for MonthBegin we can use class ***pandas.tseries.offsets.****MonthBegin(n)* for DateOffset of one month begin.

we passed n=1, so both the date values in date column is moved to the next anchor date i.e. 2022-09-01

```python
from pandas.tseries.offsets import MonthBegin

df['month_begin_date'] = df['date'] + MonthBegin(1)
df
```

|    Date    | **month_begin_date** |
| :--------: | :------------------: |
| 2022-08-31 |      2022-09-01      |
| 2022-08-24 |      2022-09-01      |

## Next Month Begin Date

For next 2 month begin date we can pass n=2, so both the date values in date column is moved to the next two anchor date i.e. 2022-10-01

```python
from pandas.tseries.offsets import MonthBegin

df['next_month_begin_date'] = df['date'] + MonthBegin(2)
df
```

|    Date    | **next_month_begin_date** |
| :--------: | :-----------------------: |
| 2022-08-31 |        2022-10-01         |
| 2022-08-24 |        2022-10-01         |
