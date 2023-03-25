---
title: "Pandas value error while merging two dataframes with different data types"
date: 2023-03-25
categories: [matplotlib, python]
tags: [matplotlib, python]
last_modified_at: 2023-03-25
permalink: pandas-value-error-while-merging-two-dataframes-with-different-data-types
---

If you're encountering a "value error" while merging Pandas data frames, this article has got you covered. Learn how to troubleshoot and solve common issues that arise during Pandas merging processes, including tips on debugging and avoiding errors in the future.

One common issue that users encounter while merging two data frames is the "value error" due to incorrect data types. This error occurs when the columns being merged have different data types, and Pandas is unable to match the values correctly. In this article, we will explore this issue in more detail and provide examples of how to resolve it

Suppose we have two data frames, one containing temperature data with two columns "latitude" and "longitude" as a string, and the other containing city name data with the columns "latitude" and "longitude" as float64. 

When we attempt to merge these two data frames on the "latitude" and "longitude" column, Pandas will throw a "value error" because the data types of the "latitude" and "longitude" columns are not the same.

## Create Dataframe

We will first create two dataframes with common columns latitude and longitude

```python
import pandas as pd

df1 = pd.DataFrame({'latitude':['40.730610', '51.509865', 
                                '13.404954', '37.618423',   
                                '35.652832', '28.644800'],
                   'longitude':['-73.935242', '-0.118092', 
                                '52.520008', '55.751244', 
                                '139.839478', '77.216721'],
                   'city':['NYC', 'London', 
                           'Berlin', 'Moscow', 
                           'Tokyo', 'Delhi'],
                   'Month':['Jan', 'Mar', 'Apr', 
                            'Aug', 'Jul', 'Dec']})

df2 = pd.DataFrame({'latitude':[40.730610, 51.509865, 
                                13.404954, 37.618423, 
                                35.652832, 28.644800],
                   'longitude':[-73.935242, -0.118092, 
                                52.520008, 55.751244, 
                                139.839478, 77.216721],
                   'temp':[53.45, 62.78, 
                           4.56, 9.87, 
                           3.24, 42.91]})
```

**df1:**

|      | latitude  | longitude  |  city  | Month |
| ---- | :-------: | :--------: | :----: | :---: |
| 0    | 40.730610 | -73.935242 |  NYC   |  Jan  |
| 1    | 51.509865 | -0.118092  | London |  Mar  |
| 2    | 13.404954 | 52.520008  | Berlin |  Apr  |
| 3    | 37.618423 | 55.751244  | Moscow |  Aug  |
| 4    | 35.652832 | 139.839478 | Tokyo  |  Jul  |
| 5    | 28.644800 | 77.216721  | Delhi  |  Dec  |

**df2:**

|      | latitude  | longitude  | Temp  |
| :--: | :-------: | :--------: | :---: |
|  0   | 40.730610 | -73.935242 | 53.45 |
|  1   | 51.509865 | -0.118092  | 62.78 |
|  2   | 13.404954 | 52.520008  | 4.56  |
|  3   | 37.618423 | 55.751244  | 9.87  |
|  4   | 35.652832 | 139.839478 | 3.24  |
|  5   | 28.644800 | 77.216721  | 42.91 |

## Verify Data type of columns in two dataframes

The latitude and longitude column in the first dataframe is type object

```python
df1.info()
```

```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5
Data columns (total 4 columns):
 #   Column     Non-Null Count  Dtype 
---  ------     --------------  ----- 
 0   latitude   6 non-null      object
 1   longitude  6 non-null      object
 2   city       6 non-null      object
 3   Month      6 non-null      object
dtypes: object(4)
memory usage: 320.0+ bytes
```

The latitude and longitude column in the second dataframe is type float64

```python
df2.info()
```

```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5
Data columns (total 3 columns):
 #   Column     Non-Null Count  Dtype  
---  ------     --------------  -----  
 0   latitude   6 non-null      float64
 1   longitude  6 non-null      float64
 2   temp       6 non-null      float64
dtypes: float64(3)
memory usage: 272.0 bytes
```

## Value Error thrown while Merging two Dataframe

So what  we see here is that the merge function throws value error if we try to merge the two dataframe(df1 and df2) on latitude and longitude

```python
df1.merge(df2, on = ['latitude', 'longitude'], suffixes = ['_left', '_right'])
```

**Out:**

```python
ValueError: You are trying to merge on object and float64 
columns. If you wish to proceed you should use pd.concat
```

## How to fix value error arises while merging two dataframe?

To fix this value error, we need to ensure the latitude and longitude column in two dataframe have same data type

We have converted the data type of two columns in dataframe(df1) to float 64

```python
df1['latitude'] = df1['latitude'].astype(float)
df1['longitude'] = df1['longitude'].astype(float)
```

Let's merge the two dataframe(df1 and df2) when latitude and longitude have the same data type

```python
df1.merge(df2, on = ['latitude', 'longitude'], suffixes = ['_left', '_right'])
```

**Out:**

|      | latitude  | longitude  |  city  | Month | temp  |
| :--: | :-------: | :--------: | :----: | ----- | ----- |
|  0   | 40.730610 | -73.935242 |  NYC   | Jan   | 53.45 |
|  1   | 51.509865 | -0.118092  | London | Mar   | 62.78 |
|  2   | 13.404954 | 52.520008  | Berlin | Apr   | 4.56  |
|  3   | 37.618423 | 55.751244  | Moscow | Aug   | 9.87  |
|  4   | 35.652832 | 139.839478 | Tokyo  | Jul   | 3.24  |
|  5   | 28.644800 | 77.216721  | Delhi  | Dec   | 42.91 |

