---

title: "Pandas search a string in dataframe across all columns"
date: "2022-02-04"
categories: [ python, pandas]
tags: [ python, pandas]

---

In this article we are going to learn how to search a string in whole dataframe across multiple columns

we will be just following these steps in order to filter out the rows where the substring contains in any cell value of dataframe in any column:

1. Create a dataframe(df)
2. Use *df.apply()* to apply string search along an axis of the dataframe and returns the matching rows
3. Use *df.applymap()* to apply string search  to a Dataframe elementwise and returns the matching rows
4. Index of all matching cells using *numpy.argwhere()*

Let's get started

## Create a dataframe
Let's create a test dataframe first, it contains two text columns(zodiac, month) and one numeric column(values). 

I am intentionally not building a dataframe with all text columns, so it could be more practical to see how we can search for a string across all columns of dataframe where one or more columns are not of type string

```python
import pandas as pd
import numpy as np


df = pd.DataFrame({'Zodiac' : ['Libra','Capricorn','Scorpio','January','Scorpio', 'Capricorn','Capricorn', 'Taurus'],
               'Month' : ['January', 'February', 'February', 'April', 'January', 'March', 'April', 'April'],
               'values' : [85, 96, 55, 64, 23, 45,89, 91]
                })
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | January |85|
| Capricorn| February|96|
| Scorpio | February | 55
| January | April | 64
| Scorpio | January | 23
| Capricorn| March|45
| Capricorn| April|89
| Taurus| April|19

Now, we will look at the dataframe functions that can be used to search for string across all the columns in a dataframe

We will search for a substring 'jan' in the dataframe and filter out the rows where this substring exists in any column 

## Using Apply to search for substring in all dataframe columns

Let's use the apply function, which is used for applying a function along an axis of the DataFrame.

we are using a lambda function to iterate over the rows of dataframe and search for substring='jan' using *str.contains()* function by casting all columns to dtype as str.


```python
substring = 'jan'
df[df.apply(lambda row: row.astype(str).str.contains(substring, case=False).any(), axis=1)]
```

So, it returns 3 rows where the substring='jan' exists in any of the 3 columns of dataframe

| zodiac| month | values|
| ---- | --- | --- |
| Libra | <font color='red'>Jan</font>uary |85|
| <font color='red'>Jan</font>uary | April | 64
| Scorpio | <font color='red'>Jan</font>uary | 23


## Using Applymap to check for substring in all dataframe columns

Next, we will use applymap function, that applies a function to a dataframe elementwise. This func returns a single value as the passed in function output

It's first parameter is a lambda function that search for substring in all column values where instance type is *str* and this returns a boolean value *True* if substring exists otherwise *False*

```python
substring = 'jan'
mask = df.applymap(lambda x: substring in x.lower() if isinstance(x,str) else False).to_numpy()
mask          
```
**Out:**

```python
array([[False,  True, False],
       [False, False, False],
       [False, False, False],
       [ True, False, False],
       [False,  True, False],
       [False, False, False],
       [False, False, False],
       [False, False, False]])
```

We will use this output to pass it thru a pandas *dataframe.loc* function that Access a group of rows and columns by a boolean array to filter out all the rows where substring exists

```python
df.loc[mask]         
```

| zodiac| month | values|
| ---- | --- | --- |
| Libra | <font color='red'>Jan</font>uary |85|
| <font color='red'>Jan</font>uary | April | 64
| Scorpio | <font color='red'>Jan</font>uary | 23



## Find index of all matching cells

So far we have seen how to filter out the rows where the substring exists in any of the columns.

Now we will see how to find the index of all the cells in dataframe(df) where the substring exists

We will use the boolean output of the applymap function above and convert that to a numpy array and pass it to the *numpy.argwhere()* function that find the indices of array elements that are non-zero

```python
substring = 'jan'
mask = df.applymap(lambda x: substring in x.lower() if isinstance(x,str) else False).to_numpy()
indices = np.argwhere(mask) 
```

The output is a 2D array with matching indices from the dataframe(df) where the substring='jan' exists

**Out:**

```python
array([[0, 1],
       [3, 0],
       [4, 1]])
```

**Conclusion:**

Here is a brief summary of this article to highlight the critical features that we learn to search for a string in a dataframe across all the columns:

- Apply function can be used with a lambda function to search for all rows in the dataframe where substring cotains in the text
- Applymap function could be used with a function that returns a single value  to output a boolean mask array that signfies the matching cells in a dataframe
- To find the indices of all the matching cells in dataframe we can use the boolean mask output and pass it in numpy.argwhere to get non-zero(True) indices

