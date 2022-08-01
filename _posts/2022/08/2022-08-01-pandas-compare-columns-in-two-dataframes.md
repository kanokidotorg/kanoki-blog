---
title: "Pandas compare columns in two data frames"
date: "2022-08-01"
categories: [ pandas, python]
tags: [ pandas, python]

---

We have two dataframes and a common column that we want to compare and find out the matching, missing values and sometimes the difference between the values using a key

We would first concatenate the two dataframes into one and see how the two dataframes looks side by side and then find out the differences between them.

However if you are interested to find the difference between two dataframes then read [this](https://kanoki.org/2019/07/04/pandas-difference-between-two-dataframes/) post

We will follow the following steps to find the difference between a column in two dataframes:

1. Create two dataframes - df1 and df2
2. Concatenate two dataframes side by side
3. Compare the column in two dataframes on a common key 
4. Additionally, find the matching rows between two dataframe
5. find the non-matching rows between the dataframes

Let's get started, we will first create two test dataframes(df1 & df2) to work upon

## Create Two Dataframes

<u>First Dataframe:</u>

The first dataframe has 2 columns: Items and Sale


```python
import pandas as pd
import numpy as np

df1 = pd.DataFrame([['A', 1], ['B', 2]],
                    columns=['Items', 'Sale'])
df1
```


|Items |Sale|
|-----|--------|
|A	|200       |
|B  |410      |

<u>Second Dataframe:</u>

The second dataframe has three columns: Items, Sale and Category

```python
df2 = pd.DataFrame([['A', 320, 'food'], ['B', 320, 'home'], ['C', 530, 'furniture']],
                    columns=['Items', 'Sale', 'Category'])
df2
```



| Items | Sale | Category  |
| ----- | ---- | --------- |
| A     | 320  | Food      |
| B     | 550  | Home      |
| C     | 530  | Furniture |



## Concatenate the two dataframes

We have concatenated the two dataframes(df1 and df2) and can see them side by side and the final concatenated dataframe is stored in variable df

```python
df=pd.concat([df1, df2],axis=1, keys = ['df1', 'df2'])
df
```


<table>
    <thead>
        <tr>
          <th></th>
            <th colspan="2">df1</th>
            <th colspan="3">df2</th>
        </tr>
        <tr>
            <th></th>
            <th>Items</th>
            <th>Sale</th>
            <th>Items</th>
            <th>Sales</th>
            <th>Category</th>          
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>A</td>
            <td>200</td>
            <td>A</td>
            <td>320</td>
            <td>Food</td>          
        </tr>  
        <tr>
	         <td>1</td>
            <td>B</td>
            <td>410</td>
            <td>B</td>
            <td>550</td>          
            <td>Home</td>          
        </tr> 
        <tr>
	         <td>2</td>
            <td>NaN</td>
            <td>NaN</td>
            <td>C</td>
            <td>530</td>
            <td>Furniture</td>
      </tr>        
    </tbody>
</table>



## Compare the columns in two dataframe

We will find the difference between the sales value between two dataframe for each of the Items

We have added a new column called as **sales-diff** to find the differences between the sales value in two dataframes where the Item values are similar otherwise difference is set to 0.

[numpy.where()](https://kanoki.org/2020/01/03/how-to-work-with-numpy-where/) is used to return choice depending on *condition*

```python
df['sales-diff']=np.where(df['df1']['Items']==df['df2']['Items'],
                    (df['df1']['Sale']-df['df2']['Sale']),
                    0)
```

We've got a new column that shows exactly the difference between the Sales column between df2 and df1

<table>
    <thead>
        <tr>
          <th></th>
            <th colspan="2">df1</th>
            <th colspan="3">df2</th>
          <th>sales-diff</th>
        </tr>
        <tr>
            <th></th>
            <th>Items</th>
            <th>Sale</th>
            <th>Items</th>
            <th>Sales</th>
            <th>Category</th>          
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>A</td>
            <td>200</td>
            <td>A</td>
            <td>320</td>
            <td>Food</td> 
          <td>120</td> 
        </tr>  
        <tr>
	         <td>1</td>
            <td>B</td>
            <td>410</td>
            <td>B</td>
            <td>550</td>          
            <td>Home</td>
          <td>140</td> 
        </tr> 
        <tr>
	         <td>2</td>
            <td>NaN</td>
            <td>NaN</td>
            <td>C</td>
            <td>530</td>
            <td>Furniture</td>
          <td>0</td> 
      </tr>        
    </tbody>
</table>

## Non-matching rows between two dataframes

Let's find the rows not matching between two dataframes(df1 and df2) based on column ***Items*** i.e. Elements of Series df1['Items'] which are not in df2['Items']

```python
df[~df['df1']['Items'].isin(df['df2']['Items'])]
```



<table>
    <thead>
        <tr>
          <th></th>
            <th colspan="2">df1</th>
            <th colspan="3">df2</th>
          <th>sales-diff</th>
        </tr>
        <tr>
            <th></th>
            <th>Items</th>
            <th>Sale</th>
            <th>Items</th>
            <th>Sales</th>
            <th>Category</th>          
        </tr>
    </thead>
    <tbody>
        <tr>
	         <td>2</td>
            <td>NaN</td>
            <td>NaN</td>
            <td>C</td>
            <td>530</td>
            <td>Furniture</td>
          <td>0</td> 
      </tr>        
    </tbody>
</table>
## Matching rows between two dataframes

We will find the rows matching between the two dataframes(df1 and df2) based on column ***Items*** i.e. Elements of Series df1['Items'] which are in df2['Items']

```python
df[df['df1']['letter']==df['df2']['letter']]
```



<table>
    <thead>
        <tr>
          <th></th>
            <th colspan="2">df1</th>
            <th colspan="3">df2</th>
          <th>sales-diff</th>
        </tr>
        <tr>
            <th></th>
            <th>Items</th>
            <th>Sale</th>
            <th>Items</th>
            <th>Sales</th>
            <th>Category</th>          
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>A</td>
            <td>200</td>
            <td>A</td>
            <td>320</td>
            <td>Food</td> 
          <td>120</td> 
        </tr>  
        <tr>
	         <td>1</td>
            <td>B</td>
            <td>410</td>
            <td>B</td>
            <td>550</td>          
            <td>Home</td>
          <td>140</td> 
        </tr>  
    </tbody>
</table>

Alternatively, we can use pandas.merge() to merge the two dataframes(df1 and df2) on column ***Items*** and apply inner join, use intersection of keys from both dataframes, similar to a SQL inner join and preserve the order of the left keys 

```python
pd.merge(df1, df2, on='Items', how='inner')
```



<table>
    <thead>        
        <tr>
            <th></th>
            <th>Items</th>
            <th>Sale_x</th>
            <th>Sale_y</th>
            <th>Category</th>          
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>A</td>
            <td>200</td>
            <td>320</td>
            <td>Food</td> 
        </tr>  
        <tr>
	         <td>1</td>
            <td>B</td>
            <td>410</td>
            <td>550</td>          
            <td>Home</td>
        </tr>  
    </tbody>
</table>
