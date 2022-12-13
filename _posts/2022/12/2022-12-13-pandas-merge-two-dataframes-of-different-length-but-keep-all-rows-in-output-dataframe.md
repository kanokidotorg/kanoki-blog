---
title: "Pandas Merge two dataframes of different length but keep all rows in output data frame"
date: 2022-12-13
categories: [ pandas, python]
tags: [ pandas, python]
last_modified_at: 2022-12-13
permalink: pandas-merge-two-dataframes-of-different-length-but-keep-all-rows-in-output-dataframe

---

In this post we will see how to merge two dataframes of unequal length on a common column or index and get all the rows from either or both the dataframes in the resulting dataframe

We will follow the following steps to get the merged output dataframe with all the rows :

1. Create two dataframes - df1 and df2 of unequal length
2. Merge two dataframe on common column
3. Get all the rows from both the dataframes
4. Get all the rows from the Left dataframe
5. Get all the rows from the Right dataframe
6. Merge two dataframe on Index and repeat step 3-5 above

Let's get started, we will first create two test dataframes(df1 & df2) to work upon

## Create Two Dataframes

<u>First Dataframe:</u>

The first dataframe has 2 columns: month and sales


```python
import pandas as pd
import numpy as np

month = ['jan', 'feb', 'mar', 'apr', 'may', 'june']
sales = [2500, 3400, 2800, 4300, 1800, 2200]

df1 = pd.DataFrame({'month': month, 'sales': sales})
```


| |month |sales|
|:---:|:------:|:---:|
|0	|jan	|2500       |
|1  |feb  |3400      |
|2 |mar |2800 |
|3 |apr |4300 |
|4 |may |1800 |
|5 |june |2200 |

<u>Second Dataframe:</u>

The second dataframe has same 2 columns as first dataframe above: month and sales

```python
month = ['feb', 'mar','aug', 'july']
sales = [1900, 1400, 2300, 4400]

df2 = pd.DataFrame({'month': month, 'sales': sales})
```



| Items | Sale | Category |
| :---: | :--: | :------: |
|   0   | feb  |   1900   |
|   1   | mar  |   1400   |
|   2   | aug  |   2300   |
|   3   | july |   4400   |



## Merge the two dataframes and keep all rows in the ouput dataframe

We will merge two dataframes(df1 and df2) on column month using parameter '*on*' and the type of merge to be performed is indicated by the parameter '*how*, which is set to outer and it's equivalent to SQL FULL *OUTER JOIN* that is used to return all of the records that have values in either the left or right tables 

```python
df = pd.merge(df1,df2, how='outer', on = 'month', suffixes = ('_left', '_right'))
df
```

**Out:**

We've used suffixes _left and _right for both the left(df1) and right(df2) dataframe respectively. You can see all the rows from two dataframes in the resulting dataframe

|      | month | sales_left | sales_right |
| :--: | :---: | :--------: | :---------: |
|  0   |  jan  |   2500.0   |     NaN     |
|  1   |  feb  |   3400.0   |   1900.0    |
|  2   |  mar  |   2800.0   |   1400.0    |
|  3   |  apr  |   4300.0   |     NaN     |
|  4   |  may  |   1800.0   |     NaN     |
|  5   | june  |   2200.0   |     NaN     |
|  6   |  aug  |    NaN     |   2300.0    |
|  7   | july  |    NaN     |   4400.0    |

## Merge the two dataframes and keep rows from Right dataframe in the ouput dataframe

We will merge two dataframes(df1 and df2) on column month and the type of merge is right here, which will keep all the rows from right(df2) dataframe only

```python
df=pd.merge(df1,df2, how='right' ,on = 'month', suffixes = ('_left', '_right'))
df
```

**Out:**

You can see the matching month rows from left(df1) dataframe and all the rows from right(df2) dataframe

|      | month | sales_left | sales_right |
| :--: | :---: | :--------: | :---------: |
|  0   |  feb  |    3400    |    1900     |
|  1   |  mar  |    2800    |    1400     |
|  2   |  aug  |    NaN     |    2300     |
|  3   | july  |    NaN     |    4400     |




## Merge the two dataframes and keep all rows from Left dataframe in the ouput dataframe

We will merge two dataframes(df1 and df2) on column month and the type of merge is left here, which will keep all the rows from left(df1) dataframe only

```python
df=pd.merge(df1,df2, how='left' ,on = 'month', suffixes = ('_left', '_right'))
df
```

**Out:**

You can see the matching month rows from right(df2) dataframe and all the rows from left(df1) dataframe

|      | month | sales_left | sales_right |
| :--: | :---: | :--------: | :---------: |
|  0   |  Jan  |    2500    |     NaN     |
|  1   |  feb  |    3400    |    1900     |
|  2   |  mar  |    2800    |    1400     |
|  3   |  apr  |    4300    |     NaN     |
|  4   |  may  |    1800    |     NaN     |
|  5   | june  |    2200    |     NaN     |

## Merge the two dataframes on Index and keep all rows from in the ouput dataframe

We will merge two dataframes(df1 and df2) on Index and the type of merge is outer here, which will keep all the rows from left(df1) and right(df2) dataframe

```python
df=pd.merge(df1,df2, how='outer' ,left_index = True, right_index = True , suffixes = ('_left', '_right'))

df
```

**Out:**

We've got 4 columns here because they are merged on index and resulting dataframe contains columns from both the df's

|      | month_left | sales_left | month_right | sales_right |
| :--: | :--------: | :--------: | :---------: | :---------: |
|  0   |    jan     |    2500    |     feb     |   1900.0    |
|  1   |    feb     |    3400    |     mar     |   1400.0    |
|  2   |    mar     |    2800    |     aug     |   2300.0    |
|  3   |    apr     |    4300    |    july     |   4400.0    |
|  4   |    may     |    1800    |     NaN     |     NaN     |
|  5   |    june    |    2200    |     NaN     |     NaN     |

