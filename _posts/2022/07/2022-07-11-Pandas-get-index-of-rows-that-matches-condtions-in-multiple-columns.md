---
title: "Pandas get row index that matches specific value in column or condition"
date: "2022-07-11"
categories: [ pandas, python]
tags: [ pandas, python]

---

We want to get index of rows that matches a specific column value or a condition based on multiple columns. The pandas index attribute get you the index of dataframe. There are several ways which can be used to get the index of the rows based on column values and conditions. 

In this post we will see all such ways that can give us index of the regular and multiindex(hierarchical index) index dataframe.

we will also use numpy functions that can give us the index directly without using the pandas index attribute of the Dataframe.

So let's create a dataset first to work with, we will create a dataframe of Metals and Prices. There are 50 rows in this data

```python
df = pd.DataFrame({'Metal':np.random.choice(['Gold', 'Platinum', 'Silver'], size=50), 'Price': np.random.randint(0, 100, 50)})
df.sample(5)
```





|      | Metal    | Price |
| ---- | -------- | ----- |
| 0    | Gold     | 88    |
| 1    | Platinum | 90    |
| 2    | Silver   | 76    |
| 3    | Gold     | 84    |
| 4    | Gold     | 74    |

### **Get row Index using Boolean Indexing**

We will use pandas popular feature which is known as Boolean indexing to get the subset of data which meets a specific value or condition based on column value 

So we want the index of only Gold and Silver from this Dataframe

```python
df[df['Metal'].isin(['Gold','Silver'])].index
```

Or we can also use .loc on the dataframe

```python
df.loc[df['Metal'].isin(['Gold','Silver'])].index
```

It returns the index of all the rows where metal column has a value either Gold or Silver.

**Output:**

```python
Int64Index([ 1,  2,  3,  5,  6,  7,  8, 10, 11, 13, 14, 17, 19, 23, 25, 26, 27,
            29, 31, 32, 37, 38, 39, 40, 41, 42, 43, 47, 48, 49],
           dtype='int64')
```

### **Get row Index using Numpy Functions**

Alternatively, we can use [numpy where](https://kanoki.org/2020/01/03/how-to-work-with-numpy-where/) and flatnonzero functions to get the row index directly based on any condition. flatnonzero function return indices that are non-zero in flattened version of array, whereas numpy *where* also returns the indices where condition is True

```python
np.where(df['Metal'].isin(['Gold','Silver']))
```

Or

```python
np.flatnonzero(df['Metal'].isin(['Gold','Silver']))
```

The output is same as the above, Returns the indices of matching rows where Metal is either Gold or Silver

**Output:**

```python
array([ 1,  2,  3,  5,  6,  7,  8, 10, 11, 13, 14, 17, 19, 23, 25, 26, 27,
       29, 31, 32, 37, 38, 39, 40, 41, 42, 43, 47, 48, 49], dtype=int64)
```

### **Get row Index using query and eval**

We could use numpy query and eval functions to evaluates a string describing operations on DataFrame column and get the rows based on conditions and then using index attribute to get the indices of all such rows in the dataframe. 

You can read more about query and eval functions in [this](https://kanoki.org/2020/01/21/pandas-dataframe-filter-with-multiple-conditions/) post.

In this case, our condition is, Metal is either Gold or Platinum and the Price is greater than 90

```python
df[df.eval('Metal == "Gold" | Metal == "Platinum" & Price > 90')].index
```

Or

```python
df.query('Metal == "Gold" | Metal == "Platinum" & Price > 90').index
```

The output is the indices of rows matching the above condition

**Output:**

```python
Int64Index([1, 3, 7, 8, 11, 13, 14, 16, 20, 21, 22, 25, 30, 31, 33, 38, 40,
            48],
           dtype='int64')
```



### Get row Index using Regex

We can also filter the rows based on a regular expression matching the value in the column. In this case we are using regex to select all rows where column Metal value starts with either character G or S

```python
df.Metal[df.Metal.str.match(r'(^G.*)|(^S.*)')==True].index
```

**Output:**

```python
Int64Index([ 1,  2,  3,  5,  6,  7,  8, 10, 11, 13, 14, 17, 19, 23, 25, 26, 27,
            29, 31, 32, 37, 38, 39, 40, 41, 42, 43, 47, 48, 49],
           dtype='int64')
```

### Get row Index in a MultiIndex dataframe

Let's create a MultiIndex dataframe first

```python
x = np.round(np.random.uniform(1, 5, size=(9, 4)), 2)

rowIndx = pd.MultiIndex.from_product(
    [["East", "North", "South"], ["A", "B", "C"]],
    names=["Region", "Division"],
)
colIndex = pd.MultiIndex.from_product(
    [["Q1", "Q2 "], ["Buy", "Sell"]]
)
multidf = pd.DataFrame(data=x, index=rowIndx, columns=colIndex)
multidf
```

|      | Region | Division | Q1-Buy | Q-Sell | Q2-Buy | Q2-Sell |
| ---- | ------ | -------- | ------ | ------ | ------ | ------- |
|      | East   | A        | 2.18   | 2.92   | 4.06   | 2.92    |
|      |        | B        | 2.44   | 2.86   | 4.79   | 2.93    |
|      |        | C        | 3.78   | 1.16   | 1.71   | 4.61    |
|      | North  | A        | 4.90   | 2.25   | 3.36   | 3.55    |
|      |        | B        | 2.14   | 4.17   | 1.97   | 3.82    |
|      |        | C        | 1.33   | 1.24   | 2.51   | 2.80    |
|      | South  | A        | 2.03   | 1.43   | 2.94   | 2.62    |
|      |        | B        | 1.83   | 3.25   | 2.93   | 2.81    |
|      |        | C        | 1.20   | 3.97   | 2.95   | 3.41    |

We want the Index for the first column level - Q1 and Q2

```python
multidf.loc[:, (multidf.columns.get_level_values(level=0)=='Q1')].index
```

It returns the MultiIndex values i.e. Region and Division for first level column

**Output:**

```python
MultiIndex([( 'East', 'A'),
            ( 'East', 'B'),
            ( 'East', 'C'),
            ('North', 'A'),
            ('North', 'B'),
            ('North', 'C'),
            ('South', 'A'),
            ('South', 'B'),
            ('South', 'C')],
           names=['Region', 'Division'])
```

Next we want to see just first level of index for this column level=0 and just the unique values

```python
multidf.loc[:, (multidf.columns.get_level_values(level=0)=='Q1')].index.get_level_values(1).unique()
```

**Output:**

```python
Index(['A', 'B', 'C'], dtype='object', name='Division')
```

Similarly for all unique index at level=0 you can change the get_level_values to zero

Next, we want the index of the rows where column Q1 - Buy is greater than 2.5, we will filter the rows where this condition is matched and then uses index attribute to get the multiIndex values

```python
multidf[multidf[( 'Q1',  'Buy')]>2.5].index
```

```python
MultiIndex([( 'East', 'C'),
            ('North', 'A'),
            ('North', 'B'),
            ('South', 'A')],
           names=['Region', 'Division'])
```



 