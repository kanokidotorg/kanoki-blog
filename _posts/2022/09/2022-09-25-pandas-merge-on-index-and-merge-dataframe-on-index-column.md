---
title: "Pandas merge on Index and merge dataframe on Index & column"
date: "2022-09-25"
categories: [python, pandas]
tags: [python, pandas]
last_modified_at: 2022-09-25
permalink: pandas-merge-on-index-or-merge-dataframe-on-index-and-column

---

In this post we will discuss how to merge two dataframes either on their Index or on Index & column, Pandas has a merge API with lot of parameters to make this job really simpler.

There are other pandas API's such as Join, concat etc. but in this post we are focusing just on merge.

Pandas DataFrame merge() function is used to merge two DataFrame objects with a database-style join operation. The joining is performed on columns or indexes where rows share the data.

We will see how to merge dataframes with different index type, A dataframe can have various type of Indexes such as Categorical, DatetimeIndex, TimeDeltaIndex, MultiIndex and many others.

## <span style="color:blue">1. Merge on Index</span>

### <span style="color:brown">a) Merge on DatetimeIndex:</span>

We want to merge our two dataframes on their DatetimeIndex, This is quite similar to a database join where you can do a left, right or outer join on one or more columns.

Let's create two dataframes with DatetimeIndex.

##### First Dataframe:

```python
df1 = pd.DataFrame(
                  {'lkey': ['foo', 
                            'bar', 
                            'baz', 
                            'foo'],
                 'value': ['Classic', 
                           'Gold', 
                           'Platinum', 
                           'Silver']},
                   index = DatetimeIndex(["2021-12-31", 
                                          "2022-01-02",
                                          "2022-03-03", 
                                          "2022-04-04"])
                  )
df1
```

|            | lkey | value    |
| ---------- | ---- | -------- |
| 2021-12-31 | foo  | Classic  |
| 2022-01-02 | bar  | Gold     |
| 2022-03-03 | baz  | Platinum |
| 2022-04-04 | foo  | silver   |

##### Second Dataframe:

```python
df2 = pd.DataFrame(
                    {'rkey': ['bar', 
                              'bar', 
                              'baz', 
                              'foo'],
                     'value': ['Classic', 
                               'Gold', 
                               'Platinum', 
                               'Silver'], },
                      index = DatetimeIndex(["2022-05-05", 
                                             "2022-02-02", 
                                             "2022-03-03", 
                                             "2022-04-04"])
                    )
df2
```

|            | lkey | value    |
| ---------- | ---- | -------- |
| 2022-05-05 | bar  | Classic  |
| 2022-02-02 | bar  | Gold     |
| 2022-03-03 | baz  | Platinum |
| 2022-04-04 | foo  | Silver   |

##### Merge - Inner Join:

We want to merge the two dataframes(df1 and df2) on their DatetimeIndex, if joining indexes on indexes or indexes on a column or columns, the index will be passed on. 

This is an inner join since we are not explicitly setting the how parameter to tell type of merge. It's the default choice.

We have passed the above two dataframes as left(df1) and right(df2) dataframe that needs to be merged.

There is a left_index and right_index parameter which is set to True, which tells to use the index from left(df1) & right(df2) dataframe respectively for the merge operation

```python
pd.merge(df1,df2,left_index=True, right_index=True)
```

The default suffix is _x and _y for left and right dataframe. The common column "value" is appended with their suffixes in the merged dataframe.

|            | key  | value_x  | rkey | value_y  |
| ---------- | ---- | -------- | ---- | -------- |
| 2022-03-03 | baz  | Platinum | baz  | Platinum |
| 2022-04-04 | foo  | Silver   | foo  | Silver   |

##### Merge - left join:

We will do a left join on index using the merge function, the how parameter is used to specify the type of merge, by default it's inner

The left merge use only keys from left frame, similar to a SQL left outer join; preserve key order.

```python
pd.merge(df1,df2,left_index=True, right_index=True, how='left', indicator=True)
```

There is a new parameter which I added here "indicator", If True, adds a column to the output DataFrame called “_merge” with information on the source of each row 

The merge column shows that the first two rows are from the left dataframe(df1)

|            | lkey | value_x  | rkey | value_y  | merge     |
| ---------- | ---- | -------- | ---- | -------- | --------- |
| 2021-12-31 | foo  | Classic  | NaN  | NaN      | left_only |
| 2022-01-02 | bar  | Gold     | NaN  | NaN      | left_only |
| 2022-03-03 | baz  | Platinum | baz  | Platinum | both      |
| 2022-04-04 | foo  | Silver   | foo  | Silver   | both      |



##  <span style="color:brown">b) Merge on Categorical Index:</span>

Let's take an another example of Dataframes with Categorical Index

##### First Dataframe:

```python
df3 = pd.DataFrame({'lkey': ['foo', 'bar', 'baz', 'foo'],
                    'value': [7, 8, 32, 51]},
                     index = CategoricalIndex(['a', 'b', 'c', 'd']))
df3
```

|      | lkey | value |
| ---- | ---- | ----- |
| a    | foo  | 7     |
| b    | bar  | 8     |
| c    | baz  | 32    |
| D    | foo  | 51    |

##### Second Dataframe:

```python
df4 = pd.DataFrame({'rkey': ['bar', 'bar', 'baz', 'foo'],
                     'value': ['a', 'b', 'e', 'f'], },
                     index=CategoricalIndex(['a', 'b', 'c', 'e']))
df4
```

|      | rkey | value |
| ---- | ---- | ----- |
| a    | bar  | a     |
| b    | bar  | b     |
| c    | baz  | e     |
| e    | foo  | f     |

##### Merge - Inner Join:

Here we want to merge the two dataframes(df3 & df4) on their CategoricalIndex, we will pass the two dataframes as parameters with left_index and right_index set=True for merge on Index

```python
pd.merge(df3,df4,left_index=True, right_index=True)
```



|      | lkey | value_x | rkey | value_y |
| ---- | ---- | ------- | ---- | ------- |
| a    | foo  | 7       | bar  | a       |
| b    | bar  | 8       | bar  | b       |
| c    | baz  | 32      | baz  | e       |

##### Merge - right join:

we want to do a right join on index using the merge function, the how parameteris set to "right" and we are passing indicator value as True

The right merge use only keys from right frame, similar to a SQL right outer join; preserve key order.

```python
pd.merge(df3,df4,left_index=True, right_index=True, how='right',indicator = True)
```

The merge column shows there is only last row which is from the right dataframe(df4)

|      | lkey | value_x | rkey | value_y | merge      |
| ---- | ---- | ------- | ---- | ------- | ---------- |
| a    | foo  | 7       | bar  | a       | both       |
| b    | bar  | 8       | bar  | b       | both       |
| c    | baz  | 32      | baz  | e       | both       |
| d    | NaN  | NaN     | foo  | f       | right_only |



## <span style="color:blue">2. Merge on Index and Column</span>

In this section, we want to merge the dataframes on Index and columns.

Let's create the two dataframes: 

```python
df5 = pd.DataFrame({'lkey': ['foo', 'bar', 'baz', 'foo'],
                 'value': [7, 8, 32, 51]})
df5
```

|      | lkey | value |
| ---- | ---- | ----- |
| 0    | foo  | 7     |
| 1    | bar  | 8     |
| 2    | baz  | 32    |
| 3    | foo  | 51    |



```python
df6 = pd.DataFrame({'rkey': ['bar', 'bar', 'baz', 'foo'],
                     'value': [0, 7, 32, 1], })
df6
```

|      | rkey | value |
| ---- | ---- | ----- |
| a    | bar  | 0     |
| b    | bar  | 7     |
| c    | baz  | 32    |
| e    | foo  | 1     |

##### Merge - Inner Join:

We want to merge on the index of left dataframe(df5) and "value" column of right dataframe(df6)

The merge API has two parameters: left_on and right_on to specify Column or index level names to join on in the left DataFrame. 

These two parameters can also be an array or list of arrays of the length of the left DataFrame. These arrays are treated as if they are columns.

```python
pd.merge(df5,df6,left_index=True, right_on='value')
```

This is an inner-join by default since we have not provided the type of merge explicitly here

|      | Value | lkey | value_x | rkey | value_y |
| ---- | ----- | ---- | ------- | ---- | ------- |
| 0    | 0     | foo  | 7       | bar  | 0       |
| 3    | 1     | bar  | 8       | foo  | 1       |

##### Merge - Outer Join

We want to perfom an Outer join on index of left dataframe(df5) and "value" column of right dataframe(df6)

We have specified the join as outer by passing it to how parameter and indicator is set to true.

```python
# Index and column -  outer join
pd.merge(df5,df6,left_index=True, right_on='value', how='outer', indicator = True)
```

The _merge column indicates the source of each row, since this is an outer join we have non-matching rows from both the dataframes

For the rows in left dataframe(df5), which doesn't have merge keys the index is NaN and also the column values of right dataframe for those rows are NaN

|      | value | lkey | value_x | rkey | value_y | _merge     |
| ---- | ----- | ---- | ------- | ---- | ------- | ---------- |
| 0.0  | 0     | foo  | 7.0     | bar  | 0.0     | both       |
| 3.0  | 1     | bar  | 8.0     | foo  | 1.0     | both       |
| NaN  | 2     | baz  | 32.0    | NaN  | NaN     | left_only  |
| NaN  | 3     | foo  | 51.0    | NaN  | NaN     | left_only  |
| 1.0  | 7     | NaN  | NaN     | bar  | 7.0     | right_only |
| 2.0  | 32    | NaN  | NaN     | baz  | 32.0    | right_only |

##### Merge - Left Join

We want to perfom a left outer join on index of left dataframe(df5) and "value" column of right dataframe(df6)

```python
# Index and column - left join
pd.merge(df5,df6,left_index=True, right_on='value', how='left', indicator = True)
```

The merge column indicates the rows which doesn't have merge keys are coming from left dataframe(df5) only.

|      | value | lkey | value_x | rkey | value_y | _merge    |
| ---- | ----- | ---- | ------- | ---- | ------- | --------- |
| 0.0  | 0     | foo  | 7.0     | bar  | 0.0     | both      |
| 3.0  | 1     | bar  | 8.0     | foo  | 1.0     | both      |
| NaN  | 2     | baz  | 32.0    | NaN  | NaN     | left_only |
| NaN  | 3     | foo  | 51.0    | NaN  | NaN     | left_only |



## <span style="color:blue">3. Merge dataframe on common column</span>

### <span style="color:brown">a) Merge using on parameter</span>

We want to merge the dataframes on common columns, pandas merge function has a "on" parameter for it

We can pass the Column or index level names to join on. These must be found in both DataFrames. 

If on is None and not merging on indexes then this defaults to the intersection of the columns in both DataFrames.

Let's see how it works, We will merge the above two dataframes(df5 & df6) on their common column "value"

```python
# Using on
pd.merge(df5,df6, on=['value'])
```

Even if we don't pass the on parameter, it will perform an inner-join on the common column

|      | lkey | value | Key  |
| ---- | ---- | ----- | ---- |
| 0    | foo  | 7     | bar  |
| 3    | baz  | 32    | Baz  |

Let's take another dataframe (df1 & df2) from above and merge "on" parameter set to the column "value" which is common column between both the dataframe

```python
# Using on
pd.merge(df1,df2, on=['value'])
```



|      | lkey | value    | Key  |
| ---- | ---- | -------- | ---- |
| 0    | foo  | Classic  | bar  |
| 1    | bar  | Gold     | bar  |
| 2    | baz  | Platinum | baz  |
| 3    | foo  | Silver   | foo  |

We can also use the `how` parameter to perform other merge operations such as left, right and outer.
