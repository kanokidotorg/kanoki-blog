---
title: "Pandas drop rows with conditions, with conditions in list and with Index and rows having NaN values"
date: 2023-01-20
categories: [pandas, python]
tags: [pandas, python]
last_modified_at: 2023-01-20
permalink: pandas-drop-rows-with-conditions-with-conditions-in-list-and-with-Index-and-rows-having-NaN-values

---

In this article, we will see how to drop rows of a Pandas dataframe based on conditions. Additionally, we will also discuss on how to drop by index, by conditions based on a list, and by NaN values. 

We will be following these steps in this article to drop rows in a dataframe based on conditions

- Create a test dataframe
- Drop rows by filtering the dataframe using boolean indexing
- Drop rows using pandas drop function 

* drop rows by condition in list
* drop rows by Index and NaN values

## Create dataframe

We will create a 15 row dataframe with student details. It has four columns - Name, Grade, Score, and Major. The dataframe column has got some NaN values too.

```python
# importing required libraries
import pandas as pd
import numpy as np

# creating dataframe
df = pd.DataFrame(
    {
        'Name':['Alex', 'David', 'Emily', 'Olivia', 'Jack', 'Kathy', 'Eric',
                'Paul', 'Mary', 'Joe', 'Jane', 'Abel', 'Sean', 'Tina', 'Laura'],
        'Grade':[8, 10, 9, 12, 11, 10, 8, 12, 11, 11, 10, 9, 9, 12, 11],
        'Score':[80, 90, 60, 70, 50, 80, 30, 90, np.nan, 10, 40, 20, 90, 70, 60],
        'Major':['Math', 'English', 'Math', 'History', 'Science', np.nan, 'History', 'Math',
                 'History', 'Math', 'English', 'Science', 'Science', 'English', 'History']
    })

# displaying the dataframe
df
```

This is how our school dataframe looks like:

|    |   Name | Grade | Score |   Major |
|---:|-------:|------:|------:|--------:|
|  0 |   Alex |     8 |  80.0 |    Math |
|  1 |  David |    10 |  90.0 | English |
|  2 |  Emily |     9 |  60.0 |    Math |
|  3 | Olivia |    12 |  70.0 | History |
|  4 |   Jack |    11 |  50.0 | Science |
|  5 |  Kathy |    10 |  80.0 |     NaN |
|  6 |   Eric |     8 |  30.0 | History |
|  7 |   Paul |    12 |  90.0 |    Math |
|  8 |   Mary |    11 |   NaN | History |
|  9 |    Joe |    11 |  10.0 |    Math |
| 10 |   Jane |    10 |  40.0 | English |
| 11 |   Abel |     9 |  20.0 | Science |
| 12 |   Sean |     9 |  90.0 | Science |
| 13 |   Tina |    12 |  70.0 | English |
| 14 |  Laura |    11 |  60.0 | History |

## Pandas drop rows by condition

We will see how to drop rows with conditions in this section. One way to achieve this is by passing integer value the other is by passing a string value. Conditions are passed with comparison and bitwise operators.

The simplest way to drop a row is by using boolean indexing and filter out the rows that doesn't met certain conditions.

* Drop all rows where score of the student is less than 80

```python
df = df[df['Score'] > 80]
```

Here, we filtered the rows by integer value of 80 and above, other rows have been dropped.

|    |  Name | Grade | Score |   Major |
|---:|------:|------:|------:|--------:|
|  1 | David |    10 |  90.0 | English |
|  7 |  Paul |    12 |  90.0 |    Math |
| 12 |  Sean |     9 |  90.0 | Science |

* Use pandas [drop](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html) function to drop the rows and df.loc will get the index of all the rows matching the condition

```python
df.drop(df.loc[df['Score'] < 80].index, axis = 0, inplace = True)
```

* Instead of Boolean Indexing you could also use query to get the index of all the rows matching conditions

```python
df.drop(df.query(" Score < 80 ").index, axis = 0, inplace = True)
```

In this, we have filtered rows which are 80 and above. Any row less than 80 is dropped.

|    |  Name | Grade | Score |   Major |
|---:|------:|------:|------:|--------:|
|  0 |  Alex |     8 |  80.0 |    Math |
|  1 | David |    10 |  90.0 | English |
|  5 | Kathy |    10 |  80.0 |     NaN |
|  7 |  Paul |    12 |  90.0 |    Math |
|  8 |  Mary |    11 |   NaN | History |
| 12 |  Sean |     9 |  90.0 | Science |

## Pandas drop rows by multiple conditions

In this section, we will see how to drop rows by multiple column values. Having multiple condition filters out the dataframe to the data that is required to run a desired operation.

* Filter the rows where grade of student is either 12 or Major is not Math using boolean indexing

```python
df = df[(df.Grade == 12) | (df.Major != 'Math')]
```

|      |   Name | Grade | Score |   Major |
| ---: | -----: | ----: | ----: | ------: |
|    1 |  David |    10 |  90.0 | English |
|    3 | Olivia |    12 |  70.0 | History |
|    4 |   Jack |    11 |  50.0 | Science |
|    5 |  Kathy |    10 |  80.0 |     NaN |
|    6 |   Eric |     8 |  30.0 | History |
|    7 |   Paul |    12 |  90.0 |    Math |
|    8 |   Mary |    11 |   NaN | History |
|   10 |   Jane |    10 |  40.0 | English |
|   11 |   Abel |     9 |  20.0 | Science |
|   12 |   Sean |     9 |  90.0 | Science |
|   13 |   Tina |    12 |  70.0 | English |
|   14 |  Laura |    11 |  60.0 | History |

* Another example of dropping the rows using the [drop](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html) function

```python
df.drop(df.loc[(df['Grade'] == 12) | (df['Major'] == 'Math') | (df['Score'] > 40)].index, axis = 0, inplace = True)
```

* You could also use query to get the index of the rows

In this we will use multiple conditions to drop specific rows.

```python
df.drop(df.query(" Grade == 12 | Major == 'Math' | Score > 40 ").index, axis = 0, inplace = True)
```

Both functions give the same output. Rows with 12th Grade Math major with the Score greater than 40 have been dropped.

|      | Name | Grade | Score |   Major |
| ---: | ---: | ----: | ----: | ------: |
|    6 | Eric |     8 |  30.0 | History |
|    8 | Mary |    11 |   NaN | History |
|   10 | Jane |    10 |  40.0 | English |
|   11 | Abel |     9 |  20.0 | Science |

## Drop rows by condition in list

In this, we create a list of values that has to be dropped. There are two ways to achieve this. We will be using `isin()` and `lambda` to achieve this.

* Drop rows by list of Index

Here, we are passing a list of index numbers to be dropped.

```python
# creating list of index numbers to dropped
x = [1, 3, 5, 7, 9, 11, 13]

df = df.drop(x)
```

|      |  Name | Grade | Score |   Major |
| ---: | ----: | ----: | ----: | ------: |
|    0 |  Alex |     8 |  80.0 |    Math |
|    2 | Emily |     9 |  60.0 |    Math |
|    4 |  Jack |    11 |  50.0 | Science |
|    6 |  Eric |     8 |  30.0 | History |
|    8 |  Mary |    11 |   NaN | History |
|   10 |  Jane |    10 |  40.0 | English |
|   12 |  Sean |     9 |  90.0 | Science |
|   14 | Laura |    11 |  60.0 | History |

* Drop rows if column values match certain list of values

To drop rows by conditions in a list we have to use [isin()](https://pandas.pydata.org/docs/reference/api/pandas.Series.isin.html). By using this function we can drop rows by their column values. This is the easiest way to drop if you know specifically which values needs to be dropped. `~` (tilde) character is the NOT operator for this function.

```python
# creating list to drop by column values
x = [8, 9, 10]

# drop rows by isin the list
df.drop(df[df.Grade.isin(x)].index, axis = 0, inplace = True)
```

* Use lambda to match the column values in a list

```python
# creating list to drop by column values
x = [8, 9, 10]

df = df.apply(lambda row: row[~df['Grade'].isin(x)])
```

Both give the same output. Rows with the list of Grades have been dropped.

|      |   Name | Grade | Score |   Major |
| ---: | -----: | ----: | ----: | ------: |
|    3 | Olivia |    12 |  70.0 | History |
|    4 |   Jack |    11 |  50.0 | Science |
|    7 |   Paul |    12 |  90.0 |    Math |
|    8 |   Mary |    11 |   NaN | History |
|    9 |    Joe |    11 |  10.0 |    Math |
|   13 |   Tina |    12 |  70.0 | English |
|   14 |  Laura |    11 |  60.0 | History |



## Drop rows by index

[drop()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html) will drops the rows by the index number.

### drop rows by index number

```python
df = df.drop(index = 1)

df = df.drop(1)
```

Both output with index[1] row dropped. We displaying the head of the dataframe.

|   |   Name | Grade | Score |   Major |
|--:|-------:|------:|------:|--------:|
| 0 |   Alex |     8 |  80.0 |    Math |
| 2 |  Emily |     9 |  60.0 |    Math |
| 3 | Olivia |    12 |  70.0 | History |
| 4 |   Jack |    11 |  50.0 | Science |
| 5 |  Kathy |    10 |  80.0 |     NaN |

Another way to drop row by index. It outputs the same as above.

```python
df = df.drop(df.index[1])
```

### drop rows by index - range

In this, we drop rows by giving the function a range. It drops all the rows mentioned in the index range. The last value is n-1.

```python
df = df.drop(df.index[range(0, 9)])
```

### drop rows by index - slice

We can drop rows by slice operation too. This is how it is done. This drops the rows from the index to index n- 1.

```python
df = df.drop(df.index[0:9])
```

Both give the same output.

|    |  Name | Grade | Score |   Major |
|---:|------:|------:|------:|--------:|
|  9 |   Joe |    11 |  10.0 |    Math |
| 10 |  Jane |    10 |  40.0 | English |
| 11 |  Abel |     9 |  20.0 | Science |
| 12 |  Sean |     9 |  90.0 | Science |
| 13 |  Tina |    12 |  70.0 | English |
| 14 | Laura |    11 |  60.0 | History |


## Drop rows with NaN values using dropna()

[`dropna()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.dropna.html) is the most efficient function when it comes to drop NaN values in a dataframe. It drops all the NaN values from the dataframe/data based on conditions and axis. We will explore `dropna()` in this section.

```python
DataFrame.dropna(*, axis=0, how=_NoDefault.no_default, thresh=_NoDefault.no_default, subset=None, inplace=False)
```

**Parameters:<br>**
*axis*: {0 or 'index', 1 or 'columns'}, default 0<br>
*how*: {'any', 'all'}, default 'any'<br>
*thresh*: int, optional<br>
*subset*: column label or sequence of labels, optional<br>
*inplace*: bool, default False<br>

```python
df = df.dropna()
```

Simply doing `dropna()` without any argument deletes all NaN values from the rows. The default axis is set at 0 that is, the rows.

|    |   Name | Grade | Score |   Major |
|---:|-------:|------:|------:|--------:|
|  0 |   Alex |     8 |  80.0 |    Math |
|  1 |  David |    10 |  90.0 | English |
|  2 |  Emily |     9 |  60.0 |    Math |
|  3 | Olivia |    12 |  70.0 | History |
|  4 |   Jack |    11 |  50.0 | Science |
|  6 |   Eric |     8 |  30.0 | History |
|  7 |   Paul |    12 |  90.0 |    Math |
|  9 |    Joe |    11 |  10.0 |    Math |
| 10 |   Jane |    10 |  40.0 | English |
| 11 |   Abel |     9 |  20.0 | Science |
| 12 |   Sean |     9 |  90.0 | Science |
| 13 |   Tina |    12 |  70.0 | English |
| 14 |  Laura |    11 |  60.0 | History |

### dropna(how)

*how*: {'any', 'all'}, default 'any'<br>
*'any'* : If any NA values are present, drop that row or column.<br>
*'all'* : If all values are NA, drop that row or column.<br>

`dropna()` has `how` parameter and the default is set to `any`. The other value is `all`. When `how = 'all'` it drops rows and columns which has all the NaN values across the dataframe. This is helpful when you are working on the data with NaN column values.

```
df = df.dropna(how = 'any')

df = df.dropna(how = 'all')
```

### dropna(subset)

`subset` lets you define the columns in which you are looking for missing values.

```
df = df.dropna(subset = ['Name', 'Grade'])
```

Since, Name and Grade columns does not have any NaN value, this will return us the whole dataframe - with the NaN values from the other columns.

### dropna(thresh)

`thresh` keeps only the rows with at least 1 non-NA value(s).

```
df.dropna(thresh = 1)
```