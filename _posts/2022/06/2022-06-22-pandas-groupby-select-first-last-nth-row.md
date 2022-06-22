---
title: "Pandas groupby and select first, last or nth row in each group"
date: "2022-06-22"
categories: [ pandas, python]
tags: [ pandas, python]

---

In this post we will see how to select specific rows in each group of pandas groupby object. There are certain cases where we are interested to select only the first, last, max, min or nth row in each group.

For example, In a dataset we would be interested to know what's  my maximum sales in each Area under a Region? or what are the top n Areas where Sale is highest? or what are my bottom n low performing Areas in terms of volume?

so let's get started, First we will create a dataframe to work upon:

```python
import pandas as pd

df=pd.DataFrame({'Region':['nordic','nordic','nordic','nordic','nordic','mid-east','mid-east',
                        'mid-east','mid-east','mid-east','south', 'south','south'],
                 'Area':[1,1,1,2,2,1,2,1,3,3,1,2,3],'Sales':[10,15,21,36,20,42,36,52,18,25,13,35,23]})
df
```
There are three columns in this dataframe Region, Area and Sales.

**Output:**

| Region      |     Area    |   Sales   |
| ----------- | ----------- |-----------|
| nordic      | 1           |  10 |
| nordic      | 1           |  15 |
| nordic      | 1           |  21 |
| nordic      | 2           |  36 |
| nordic      | 2           |  20 |
| mid-east      | 1           |  42 |
| mid-east      | 2           |  36 |
| mid-east      | 1           |  52 |
| mid-east      | 3           |  18 |
| mid-east      | 3           |  25 |
| south      | 1           |  13 |
| south      | 2           |  35 |
| south      | 3           |  23 |

### Select rows with max or min value of sale sum in each Area under a Region

In this section we are interested to find the max, min rows from each group and we will first groupby Region, Area and sum up the sales

```python
df_agg=df.groupby(['Region','Area']).agg({'Sales':sum})
df_agg
```
So you can see the below dataframe grouped by Region and Area and the total sum for each Area under a Region

| Region      |     Area    |   Sales   |
| ----------- | ----------- |-----------|
| mid-east      | 1           |  94|
|      | 2           |  36|
|      | 3           |  43|
|    nordic  | 1           |  46|
|      | 2           |  56|
| south      | 1           |  13 |
|      | 2           |  35 |
|      | 3           |  23 |

There are couple of ways to find out the maximum and minimum of total sales in each Area, we will first groupby Region and sort the rows in each group in ascending order and get the last row(tail(1)) which will be maximum and the first row(head(1)) will be minimum

```python
g=df_agg['Sales'].groupby('Region', group_keys=False)
g.apply(lambda x: x.sort_values(ascending=True).tail(1))
```

you can see the maximum sales is returned in each row by using tail(1)

| Region      |     Area    |     |
| ----------- | ----------- |-----------|
| mid-east      | 1           |  94|
| nordic      | 2           |  56|
| south      | 1           |  35|


```python
g=df_agg['Sales'].groupby('Region', group_keys=False)
g.apply(lambda x: x.sort_values(ascending=True).head(1))
```
And the  minimum sales from each group is returned using head(1)

| Region      |     Area    |     |
| ----------- | ----------- |-----------|
| mid-east      | 2           |  36|
| nordic      | 1           |  46|
| south      | 1           |  13|


### Alternate way to find first, last and min,max rows in each group

Pandas has first, last, max and min functions that returns the first, last, max and min rows from each group

For computing the first row in each group just groupby Region and call first() function as shown below

```python
df_agg=df.groupby(['Region','Area']).agg({'Sales':sum})
g=df_agg.groupby('Region', group_keys=False)['Sales'].first()
```
**Output:**

| Region      |     Area    |     |
| ----------- | ----------- |-----------|
| mid-east      | 1           |  94|
| nordic      | 2           |  46 |
| south      | 1           |  13 |

Note: this is the first row in each group and not the max sales value

Similarly for the last row, you can use the last() function instead of first()

Now to return the max and min rows in each group you can just replace the first function above with the max or min as shown here

```python
df_agg=df.groupby(['Region','Area']).agg({'Sales':sum})
g=df_agg.groupby('Region', group_keys=False)['Sales'].max()
```
**Output:**

| Region      |     Area    |     |
| ----------- | ----------- |-----------|
| mid-east      | 1           |  94|
| nordic      | 2           |  56|
| south      | 1           |  35|

### How to find nth row in each group

Pandas has an in-built function nth that take the nth row from each group if n is an integer, otherwise a subset of rows

Alright, let's find get the first and last row from our group by object above

```python
# both first and last row
df_agg=df_agg.reset_index()
df_agg.groupby('Region', group_keys=False)['Sales'].nth([0,-1])
```
You can see only the first and last row is returned from each group

**Output:**

| Region      |     Area    |     |
| ----------- | ----------- |-----------|
| mid-east      | 1           |  94|
| mid-east      | 3           |  43|
| nordic      | 1           |  46|
| nordic      | 2           |  56|
| south      | 1           |  13|
| south      | 3           |  23|

You can also get the subset of rows which will be useful when you are looking for top 5 or 3 rows in each group, you can sort the values in each group and then slice it. 

Here is an example to get a subset of rows, in this case we want the bottom 2 Sales from each group

```python
g=df_agg.groupby('Region', group_keys=False)
g.apply(lambda x: x.sort_values('Sales',ascending=True)).groupby('Region', group_keys=False).nth[:2]
```
**Output:**

| Region      |     Area    |  Sales   |
| ----------- | ----------- |-----------|
| mid-east      | 2           |  36|
| mid-east      | 3           |  43|
| nordic      | 1           |  46|
| nordic      | 2           |  56|
| south      | 1           |  13|
| south      | 3           |  23|