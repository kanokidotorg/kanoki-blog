---
title: "Pandas convert column into rows and transpose rows to column"
date: 2022-11-07
categories: [python, pandas]
tags: [python, pandas]
last_modified_at: 2022-11-07
permalink: pandas-convert-column-into-rows-and-transpose-rows-to-column
---

In this post we will see how to convert the column into rows, rows into columns, transpose one column into multiple columns and how to Pivot/Unpivot the dataframe using the following useful pandas functions:

- Melt
- Pivot and Pivot_table
- stack and Unstack
- Wide_to_long

The above functions are useful to massage a DataFrame and rotates data from rows to columns or vice-versa, possibly aggregating multiple source values into the same target row and column intersection

This post will be helpful for someone who want to format the data from wide to long or long to wide for any reporting purpose or computing the custom aggrgations

## <span style="color:blue">Convert column into rows</span>

Let's create a dataframe of Quarterly sales figure of different locations

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
'location': ['AL', 'TX', 'MO'],
'area': ['JN', 'AU', 'JC'],
'q1': [44, 25, 36],
'q2': [29, 32, 39],
'q3': [34, 43, 24],})

df
```

|      | location | Area | Q1   | Q2   | Q3   |
| ---- | -------- | ---- | ---- | ---- | ---- |
| 0    | AL       | JN   | 44   | 29   | 34   |
| 1    | TX       | AU   | 25   | 32   | 43   |
| 2    | MO       | JC   | 36   | 39   | 24   |



### <span style="color:brown">a) pd.melt()</span>

We want to unpivot the above dataframe and convert the columns into rows, Basically we want to format this dataframe from wide to long, We can use pandas.melt() function to achive this

```python
df.melt(id_vars=["location", "area"], 
        var_name="quarter", 
        value_name="sales")
  .sort_values(['location', 'quarter'])
  .reset_index(drop=True)
```

**What are the parameters of the melt function?**

*Id_vars:* It is a list of column names that will repeated for all the quarterly sales and that's why they are got ID variables that represent the sales figure i.e. location and area in this case

*Var_name:* The new column name that will be created after reshaping the dataframe, In this case we want to name it quarter and it will have the values as q1, q2 and q3

*Value_name:* The name of the value column, we want to name this column as sales for this dataframe

Out:

Here is the reshaped output of the dataframe:

|      | location | area | quarter | sales |
| :--: | :------: | :--: | :-----: | :---: |
|  0   |    AL    |  JN  |   q1    |  44   |
|  1   |    AL    |  JN  |   q2    |  29   |
|  2   |    AL    |  JN  |   q3    |  34   |
|  3   |    MO    |  JC  |   q1    |  36   |
|  4   |    MO    |  JC  |   q2    |  39   |
|  5   |    MO    |  JC  |   q3    |  24   |
|  6   |    TX    |  AU  |   q1    |  25   |
|  7   |    TX    |  AU  |   q2    |  32   |
|  8   |    TX    |  AU  |   q3    |  43   |

### <span style="color:brown">b) df.stack()</span>

We could also use stack to get the same result as above, the stack function stack the prescribed level(s) from columns to index.

It returns a reshaped DataFrame having a multi-level index with one or more new inner-most levels compared to the original dataFrame

```python
(df.set_index(["location", "area"])
         .stack()
         .reset_index(name='sales')
         .rename(columns={'level_2':'quarter'}))
```

### <span style="color:brown">c) pd.wide_to_long()</span>

we could also use wide_to_long() function to Unpivot a DataFrame from wide to long format. It is less flexible but more user-friendly than melt

```python
(pd.wide_to_long(df, stubnames=['q'], i='location', j='quarter')
.reset_index().sort_values(['location', 'quarter'])
.rename(columns={'q':'sales'}))
```

**What are the parameters of the wide_to_long function?**

*stubnames:* The name of the new column that represent one or more group of columns with format in the original dataframe, In our case the group of columns are Q1,Q2,Q3

*i:* Each row of these wide variables are assumed to be uniquely identified by i, It can be a single or list of column names

*j:* It is the name of the column that specify what you want to call the suffix in the stubnames, In this case Q1, Q2, Q3 represent the Quarter

Out:

Here is the resulting dataframe, we've renamed the column q to sales to make the dataframe looks similar to the above output dataframe

|      | location | quarter | area | Sales |
| :--: | :------: | :-----: | :--: | :---: |
|  0   |    AL    |    1    |  JN  |  44   |
|  3   |    AL    |    2    |  JN  |  29   |
|  6   |    AL    |    3    |  JN  |  34   |
|  2   |    MO    |    1    |  JC  |  36   |
|  5   |    MO    |    2    |  JC  |  39   |
|  8   |    MO    |    3    |  JC  |  24   |
|  1   |    TX    |    1    |  AU  |  25   |
|  4   |    TX    |    2    |  AU  |  32   |
|  7   |    TX    |    3    |  AU  |  43   |

## <span style="color:blue">Transpose one column into multiple columns</span>

We want to pivot the resulting dataframes above back to their original dataframes, which is converting the rows into columns.

Let's create a dataframe similar to the original dataframe above

```python
df = pd.DataFrame({
'location': ['TX', 'TX', 'TX'],
'area': ['HOU', 'PAL', 'DAL'],
'sales': [72, 59, 63],})
```

|      | location | area | sales |
| :--: | :------: | :--: | :---: |
|  0   |    TX    | HOU  |  72   |
|  1   |    TX    | PAL  |  59   |
|  2   |    TX    | DAL  |  63   |

### <span style="color:brown">a) pd.pivot()</span>

we can pivot this dataframe using pivot_table or pivot function that will create a spreadsheet-style pivot table as a DataFrame and the data is transformed and stored in MultiIndex objects (hierarchical indexes) on the index and columns of the result DataFrame

```python
df.pivot_table(index='location', columns='area', values='sales')

#OR

df.pivot(index='location', columns='area', values='sales')
```

**What are the parameters of the pivot() function?**

*Index:* grouper or the column which you want to aggregate

*Columns:* grouper or the column, to be converted from rows to columns

*Value:* The value of the Index and Column that you want to display

**Out:**

Output Dataframe

|     area     | DAL  | HOU  | PAL  |
| :----------: | :--: | :--: | :--: |
| **location** |      |      |      |
|      TX      |  63  |  72  |  59  |

### <span style="color:brown">b) pd.unstack()</span>

we can pivot at the level of index labels using unstack() function. It returns a DataFrame having a new level of column labels whose inner-most level consists of the pivoted index labels

```python
df.set_index(['location','area']).unstack()
```

**Out:**

Output Dataframe

|              | sales |      |      |
| :----------: | :---: | :--: | :--: |
|     area     |  DAL  | HOU  | PAL  |
| **location** |       |      |      |
|      TX      |  63   |  72  |  59  |
