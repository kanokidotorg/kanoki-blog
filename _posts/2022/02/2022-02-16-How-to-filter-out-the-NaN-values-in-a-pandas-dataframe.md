---

title: "How to filter out the NaN values in a pandas dataframe"
date: "2022-02-16"
categories: [ python, pandas]
tags: [ python, pandas]

---

As data comes in many shapes and forms, Missing values in pandas are denoted as NaN, It is a special floating-point value. There are also other ways to represent the missing data like Python None which is considered as “missing” or “not available” or “NA”

we will see how to filter out the NaN values in a data using different techniques in pandas:

1. Create a dataframe with at least one NaN values in all the columns
2. Use *dataframe.notnull()* *dataframe.dropna()* to filter out all the rows with a NaN value
3. Use *Series.notna()* and *pd.isnull()* to filter out the rows where NaN is present in a particular column of dataframe
4. Use *dataframe.dropna()* to drop rows and columns of a dataframe based on the axis value as 0 or 1 and additionally we will see how to setup threshold value for dropping the NaN's

## Create a dataframe
Let's create a dataframe with at lease one NaN value in each column

```python
import numpy as np
import pandas as pd

df = pd.DataFrame({'Zodiac' : ['Libra','Capricorn','Scorpio','Taurus','Scorpio', 'Capricorn','Capricorn', np.nan, 'Taurus'],
               'Month' : ['January', 'February', 'February', np.nan, 'January', 'March', 'April', 'April', np.nan],
               'values' : [85, 96, np.nan, 64, 23, 45, np.nan, 89, 91]
                })
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|
| Capricorn| February|96|
| Scorpio | February | <font color='red'>NaN</font>|
| Taurus | <font color='red'>NaN</font> | 64|
| Scorpio | January | 23|
| Capricorn| March|45|
| Capricorn| April|<font color='red'>NaN</font>|
| <font color='red'>NaN</font>| April|89|
| Taurus| <font color='red'>NaN</font>|91|


## Filter out all rows with NaN value in a dataframe

We will filter out all the rows in above dataframe(df) where a NaN value is present

*dataframe.notnull()* detects existing (non-missing) values and returns a boolean same-sized object indicating if the values are not NA. Non-missing values get mapped to True and NA values, such as None or numpy.NaN, get mapped to False values

We use *dataframe.notnull()* with *all()* that will return True whether all elements are True, potentially over an axis

Alternatively, we can also use *dataframe.dropna()* to remove missing values and returns a DataFrame with NA entries dropped from it or None if inplace=True

```python
df[df.notnull().all(1)]

#or 

df.dropna()
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|
| Capricorn | February | 96|
| Scorpio | January | 23|
| Capricorn | March | 45|


## drop rows with NaN in specific column

To drop rows where NA or NaN value is available in a particular column(values in our df) we can use *notnull(), notna()* and *pd.isnull()*

*Series.notna()* detects existing (non-missing) values and is an alias for *Series.notnull()*

Whereas, *pd.isnull()* detects missing values for an array-like object, It takes a scalar or array-like object and indicates whether values are missing (NaN in numeric arrays, None or NaN in object arrays) and returns an array of boolean indicating whether each corresponding element is missing

```python
df[df['values'].notnull()]

#or

df[df['values'].notna()]

#or

df[~pd.isnull(df['values'])]         
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|
| Capricorn | February | 96|
| Taurus | NaN | 64|
| Scorpio | January |23|
| Capricorn | March | 45|
| NaN | April| 89|
| Taurus | NaN| 91|




## drop rows with NaN in multiple columns

*dataframe.dropna()* has a subset parameter that takes list of columns to include for dropping rows 

```python
cols = ['values','Month']
df[df[cols].notnull().all(1)]

#or

df.dropna(subset=cols)
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|
| Capricorn | February | 96|
| Scorpio | January |23|
| Capricorn | March | 45|
| NaN | April| 89|


## Create a Dataframe with a NaN column
Let's create another dataframe with one of the column(others) as NaN

```python
df = pd.DataFrame({'Zodiac' : ['Libra','Capricorn','Scorpio','Taurus','Scorpio', 'Capricorn', np.nan, np.nan, 'Taurus'],
               'Month' : ['January', 'February', 'February', np.nan, 'January', 'March',  np.nan,  np.nan,'December'],
               'values' : [85, 96, np.nan, 64, 23, 45, np.nan,  np.nan, 91],
                   'others': [np.nan]*9
                })
df
```

| zodiac| month | values|others|
| ---- | --- | --- |--- |
| Libra | January |85|NaN|
| Capricorn| February|96|NaN|
| Scorpio | February | NaN|NaN|
| Taurus | NaN| 64|NaN|
| Scorpio | January | 23|NaN|
| Capricorn| March|45|NaN|
| NaN | NaN |NaN|NaN|
| NaN| NaN | NaN |NaN|
| Taurus| December|91|NaN|


### drop columns with all NaN values

if we have to drop this column(others) with all missing values then *dataframe.dropna()* has a parameter *how* that let you drop that row or column where all values are NA or NaN along the given axis

Here we are setting axis=1, which means to drop the column where all values are NA

```python
df.dropna(how='all',axis=1)
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|
| Capricorn| February|96|
| Scorpio | February | NaN|
| Taurus | NaN| 64|
| Scorpio | January | 23|
| Capricorn| March|45|
| NaN | NaN |NaN|
| NaN| NaN | NaN |
| Taurus| December |91|

### drop rows with all NaN values

Just pass the axis=0 to drop rows where all values are NA or NaN

```python
df.dropna(how='all',axis=0)
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|NaN|
| Capricorn| February|96|NaN|
| Scorpio | February | NaN|NaN|
| Taurus | NaN| 64|NaN|
| Scorpio | January | 23|NaN|
| Capricorn| March|45|NaN|
| Taurus| December|91|NaN|

### drop rows with a threshold parameter

Finally, you can also set the minimum number of non-NA values required to drop a row or column by passing the *thresh* parameter, see this below example with thresh=1 and axis=0(rows)

```python
df.dropna(thresh=1, axis=0)
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|NaN|
| Capricorn| February|96|NaN|
| Scorpio | February | NaN|NaN|
| Taurus | NaN| 64|NaN|
| Scorpio | January | 23|NaN|
| Capricorn| March|45|NaN|
| Taurus| December|91|NaN|