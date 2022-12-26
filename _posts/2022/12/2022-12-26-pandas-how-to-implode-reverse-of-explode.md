---
title: "How to implode in pandas - reverse of explode"
date: 2022-12-26
categories: [ pandas, python]
tags: [ pandas, python]
last_modified_at: 2022-12-26
permalink: pandas-implode-reverse-of-explode
---

In this post we are going to see how to perform reverse of explode

We will be following the below steps to implode a column in the dataframe:

1. Create a dataframe
2. Group the dataframe using desired columns 
3. Use Aggregate function to create list of values in a column for each group

## Create Dataframe

Let's create a dataframe with five columns - Area, Sales_million, Month, Gallons and Rank

```python
df=pd.DataFrame({'Area': ['North America']*5,
                 'Region': ['Midwest', 'Northeast', 
                            'Southwest', 'Midwest', 
                            'Northeast']
             'Sales_million': [35, 45, 50, 45, 50],
             'Month': ['Jan', 'Feb', 'Mar', 'Apr', "May"],
             'Gallons': [200, 215, 320, 220, 195],
             'Rank': [5, 2, 1, 4, 3]})
df
```

**Out:**

|      |     Area      | Region    | Sales_million | Month | Gallons | Rank |
| :--: | :-----------: | --------- | :-----------: | :---: | :-----: | ---- |
|  0   | North America | Midwest   |      35       |  Jan  |   200   | 5    |
|  1   | North America | Northeast |      45       |  Feb  |   215   | 2    |
|  2   | North America | Southwest |      50       |  Mar  |   320   | 1    |
|  3   | North America | Midwest   |      45       |  Apr  |   220   | 4    |
|  4   | North America | Northeast |      50       |  May  |   195   | 3    |

## Use Group by to aggregate columns 

We want to group the dataframe on Area and Region and aggregate the values in column Gallons, Rank and Sales_million, and combine the value of Month in all rows into a single list

we will pass the *list* function for Month column in Groupby aggregate function, It will combine the value of Month column in one single list. 

**a) Aggregate columns and combine the values as list**

```python
(df.groupby(['Area','Region'])
      .agg({'Month': list,
            'Gallons':'mean',
            'Rank':'mean', 
            'Sales_million': 'sum'})
      .reset_index())
```

**Out:**

|      | Area          |  Region   | Month      | Gallons | Rank | Sales_million |
| :--: | ------------- | :-------: | ---------- | ------- | :--: | :-----------: |
|  0   | North America |  Midwest  | [Jan, Apr] | 210     | 4.5  |      80       |
|  1   | North America | Northeast | [Feb, May] | 205     | 2.5  |      95       |
|  2   | North America | Southwest | [Mar]      | 320     | 1.0  |      50       |



**b) Rename columns**

We will rename the columns to make it more meaningful and self explanatory. 

NamedAggregation is used to  to make it clearer what the arguments are and to control the output names with different aggregations per column

The values are tuples whose first element is the column to select and the second element is the aggregation to apply to that column.

```python
df1=(df.groupby(['Area','Region'])
      .agg(
         Month_list = 
            pd.NamedAgg(column = 'Month', 
                        aggfunc = list),
         Avg_gallon = 
            pd.NamedAgg(column = 'Gallons', 
                        aggfunc = 'mean'),
         Avg_rank = 
            pd.NamedAgg(column = 'Rank', 
                        aggfunc = 'mean'),
         Sum_sales_million = 
            pd.NamedAgg(column = 'Sales_million', 
                        aggfunc = 'sum')
      )
      .reset_index())
df1
```

**Out:**

|      | Area          | Region    | Month_list | Avg_gallon | Avg_rank | Sum_sales_million |
| :--: | ------------- | --------- | ---------- | ---------- | :------: | :---------------: |
|  0   | North America | Midwest   | [Jan, Apr] | 210        |   4.5    |        80         |
|  1   | North America | Northeast | [Feb, May] | 205        |   2.5    |        95         |
|  2   | North America | Southwest | [Mar]      | 320        |   1.0    |        50         |



**b) Combine the values across multiple columns as list**

we want to combine the values of Avg_gallon, Avg_rank and Sum_sales_million into a single list. In short we want to combine the values of three columns into a single list horizontally

```python
cols = ['Avg_gallon', 'Avg_rank', 'Sum_sales_million']
df1['group_gallon_rank_sales'] = df1[cols].values.tolist()
df1.drop(cols, axis=1)
```

**Out:**

|      |     Area      |  Region   | Month_list | group_gallon_rank_sales |
| :--: | :-----------: | :-------: | :--------: | :---------------------: |
|  0   | North America |  Midwest  | [Jan, Apr] |   [210.0, 4.5, 80.0]    |
|  1   | North America | Northeast | [Feb, May] |   [205.0, 2.5, 95.0]    |
|  2   | North America | Southwest |   [Mar]    |   [320.0, 1.0, 50.0]    |

