---
title: "Pandas combine two text columns"
date: 2022-11-12
categories: [python, pandas]
tags: [python, pandas]
last_modified_at: 2022-11-12
permalink: Pandas-combine-two-text-columns

---

In this post we will learn how to concatenate two or more string columns of a dataframe. You can use either + operator or the str.cat() function to combine two or more column values

We will also see how to combine columns based on conditions, Also how to combine columns with Nulls or NaN values.

## <span style="color:blue">Concatenate multiple column values into one column</span>

Let's create a dataframe with two text columns author and book_name


```python
import numpy as np
import pandas as pd


data = {'book_id': [1, 2, 3, 4, 5],
         'author': ['J.R.R. Tolkien', 'J.D. Salinger', np.nan, ' F. Scott Fitzgerald', np.nan],
         'book_name': ['The Lord of the Rings', 'The Catcher in the Rye', 'To Kill a Mockingbird', np.nan, np.nan]
        }

df = pd.DataFrame(data)

df
```

This is how our dataframe looks like

|      | book_id |              author |              book_name |
| ---: | ------: | ------------------: | ---------------------: |
|    0 |       1 |      J.R.R. Tolkien |  The Lord of the Rings |
|    1 |       2 |       J.D. Salinger | The Catcher in the Rye |
|    2 |       3 |                 NaN |  To Kill a Mockingbird |
|    3 |       4 | F. Scott Fitzgerald |                    NaN |
|    4 |       5 |                 NaN |                    NaN |

Let's combine the author and book_name column of the above dataframe.

There are two ways you can combine two or more text columns into a single column.

### <span style="color:brown">1. + operator (plus operator)</span>

```python
df['book_details'] = df['author'] + df['book_name']
```
**Note:** + operator performs addition in case of two number values.

### <span style="color:brown">2. pandas.series.str.cat() function:</span>
```python
df["book_details"] = df["author"].str.cat( df["book_name"] )
```
Parameters of str.cat()

*others*: Series, Index, DataFrame, np.ndarray or list-like. When others is None, the method returns the concatenation of all strings in the calling series/index.

*sep*: default sep is set to '' i.e. no space

*na_rep*: to replace null values; default is set to None

*join*: {‘left’, ‘right’, ‘outer’, ‘inner’}, default ‘left’<br>
There are 4 attributes to join. This determines the join-style between the calling series/index/dataframe.

**Note:** + Operator is convenient when you are working with a large dataset whereas series.str.cat() is used when a dataset is relatively small.

Here's how the dataframe looks after concatenating the two text columns. 

Both the above code gives the same output.

|   | book_id |              author |              book_name |                        book_details |
|--:|--------:|--------------------:|-----------------------:|------------------------------------:|
| 0 |       1 |      J.R.R. Tolkien |  The Lord of the Rings | J.R.R. TolkienThe Lord of the Rings |
| 1 |       2 |       J.D. Salinger | The Catcher in the Rye | J.D. SalingerThe Catcher in the Rye |
| 2 |       3 |                 NaN |  To Kill a Mockingbird |                                 NaN |
| 3 |       4 | F. Scott Fitzgerald |                    NaN |                                 NaN |
| 4 |       5 |                 NaN |                    NaN |                                 NaN |

The default separator set in both cases '' i.e. no space. We will learn more about separators next.


## <span style="color:blue">Combine non-string columns</span>

In this section we will learn how to combine a string and a non-string value in pandas, we will use the **astype(str)** and **map(str)** methods. 

The .astype(str) converts the column value into string, map(str) does the same.

 Null/NaN values are considered float so, to combine columns with null values we have to first change their datatype.

### .astype(str)
.astype(str) is used to cast a pandas object to a specified datatype (dtype).

```python
df['book_details'] = df['book_id'].astype(str) + '-' + df['author'].astype(str) + '-' + df['book_name'].astype(str)

```

### .map(str)
.map(str) is used for substituting each value in a series with another value, that may be derived from a function, a dict, or a series.

```python
df['book_details'] = df['book_id'].map(str) + '-' + df['author'].map(str) + '-' + df['book_name'].map(str)
```

Both astype(str) and map(str) give the same output in our case.

|                           book_details |
|---------------------------------------:|
| 1-J.R.R. Tolkien-The Lord of the Rings |
| 2-J.D. Salinger-The Catcher in the Rye |
|            3-nan-To Kill a Mockingbird |
|             4- F. Scott Fitzgerald-nan |
|                              5-nan-nan |


### join()
join() function is used to join strings. It is used paired with other concatenating functions.

### .agg()
.agg() is used to join multiple string columns. It combines all the columns as a list.
```python
df['book_details'] = df[['author', 'book_name']].fillna('None').agg('-'.join, axis=1)
```
*axis*: 0 or index; default, 1 or column
### .apply() and .join()
apply() is to apply function when combining two or more columns into a single column. It is used to apply another function on a specific axis.
```python
df['book_details'] = df[['author', 'book_name']].fillna('None').apply("-".join, axis=1)
```

### .apply() with lambda
apply() with lambda can be used to achieve the same with any column slice of your dataframe.
```python
df['book_details'] = df[['author', 'book_name']].fillna('None').apply(lambda x: "-".join(x), axis =1)
```
All three functions give the same output

|                         book_details |
|-------------------------------------:|
| J.R.R. Tolkien-The Lord of the Rings |
| J.D. Salinger-The Catcher in the Rye |
|           None-To Kill a Mockingbird |
|             F. Scott Fitzgerald-None |
|                            None-None |

## <span style="color:blue">Combine columns with conditions</span>

Let's combine the columns based on conditions, We want to combine the columns where any of the columns doesn't have a NaN value in it.

We are using np.where. You can read more about it in [this](https://kanoki.org/2020/01/03/how-to-work-with-numpy-where/) article.

```python
df['book_details'] = np.where(((df.author.notnull())&(df.book_name.notnull())),(df['author'].astype(str)+df['book_name'].astype(str)),np.nan)
```

We've combined only the rows where both author and book_name field has values otherwise the book_details has a NaN value

|                        book_details |
| ----------------------------------: |
| J.R.R. TolkienThe Lord of the Rings |
| J.D. SalingerThe Catcher in the Rye |
|                                 NaN |
|                                 NaN |
|                                 NaN |

## <span style="color:blue">Combine columns with nulls/nans</span>

In this section, we will learn ways of combining string columns with null values. 

One way is by using str.cat() parameter. And the other is by using fillna(). 

This dataframe has some missing values in the second and third columns. 

We will learn how to combine a value + null and null + null string columns in this section.

Normally when two string columns with null values are combined it doesn't add any column. The output is NaN.

### na_rep parameter

One way to replace null values is by using na_rep parameter of str.cat() function. 

We can assign na_rep with the string value and replace all the null values and combine the columns.


```python
df["book_details"] = df['author'].str.cat(df['book_name'], sep = '-', na_rep = 'None')
```
All the NaN values have been replaced by string values and concatenated.

|                         book_details |
|-------------------------------------:|
| J.R.R. Tolkien-The Lord of the Rings |
| J.D. Salinger-The Catcher in the Rye |
|           None-To Kill a Mockingbird |
|             F. Scott Fitzgerald-None |
|                            None-None |

### fillna()

fillna() replaces all null values with a specified string. It is useful when the data is large and unkempt. 

fillna() parameters are useful to target specific columns or rows. It also has different methods to replace the null values.

```python
DataFrame.fillna(value=None, *, method=None, axis=None, inplace=False, limit=None, downcast=None)
```

#### fillna() - string
```python
df["book_details"] = df["author"].fillna('None') + ", " + df["book_name"].fillna('None')
```

|                          book_details |
|--------------------------------------:|
| J.R.R. Tolkien, The Lord of the Rings |
| J.D. Salinger, The Catcher in the Rye |
|           None, To Kill a Mockingbird |
|             F. Scott Fitzgerald, None |
|                            None, None |

#### fillna() - column data
```python
df["name"] = df["first_name"].fillna('None') + " " + df["last_name"].fillna(df['first_name'])
```


|                             book_details |
|-----------------------------------------:|
|    J.R.R. Tolkien, The Lord of the Rings |
|    J.D. Salinger, The Catcher in the Rye |
|              None, To Kill a Mockingbird |
| F. Scott Fitzgerald, F. Scott Fitzgerald |
|                                      NaN |

#### fillna() with method

Two methods for the filling of value:

*pad/ffill*: forward fill and *bfill/backfill*: back fill

*default is set to None*

```python
df["book_details"] = df["author"].fillna(method = 'bfill') + ", " + df["book_name"].fillna(method = 'ffill')
```


|                               book_details |
|-------------------------------------------:|
|      J.R.R. Tolkien, The Lord of the Rings |
|      J.D. Salinger, The Catcher in the Rye |
| F. Scott Fitzgerald, To Kill a Mockingbird |
| F. Scott Fitzgerald, To Kill a Mockingbird |
|                                        NaN |

#### fillna() with limit
```python
df["book_details"] = df["author"].fillna(method = 'bfill', limit = 1) + ", " + df["book_name"].fillna('None', limit = 1)
```


|                               book_details |
|-------------------------------------------:|
|      J.R.R. Tolkien, The Lord of the Rings |
|      J.D. Salinger, The Catcher in the Rye |
| F. Scott Fitzgerald, To Kill a Mockingbird |
|                  F. Scott Fitzgerald, None |
|                                        NaN |


## <span style="color:blue">Combine columns with separators/delimiters</span>

Separators/Delimiters are used to separate the different column values in a row. Separators are also called Delimiters. 

They are always treated as strings and enclosed within quotes. space is the default value of the Separator parameter

Following are different separators used in pandas:

- default '' (no space)
- space '   '
- comma ' , '
- hyphen ' - '
- pipe ' \| '
- colon ' : '
- semi-colon ' ; '


```python
# using + operator
df['book_details'] = df['author'] + '-' + df['book_name']

# using str.cat()
df['book_details'] = df['author'].str.cat(df['book_name'], sep = '-')
```

Both of the above codes will give the same output.

|   | book_id |              author |              book_name |                         book_details |
|--:|--------:|--------------------:|-----------------------:|-------------------------------------:|
| 0 |       1 |      J.R.R. Tolkien |  The Lord of the Rings | J.R.R. Tolkien-The Lord of the Rings |
| 1 |       2 |       J.D. Salinger | The Catcher in the Rye | J.D. Salinger-The Catcher in the Rye |
| 2 |       3 |                 NaN |  To Kill a Mockingbird |                                  NaN |
| 3 |       4 | F. Scott Fitzgerald |                    NaN |                                  NaN |
| 4 |       5 |                 NaN |                    NaN |                                  NaN |

Here are a few examples of how it looks:
#### Separator set to comma ,
```python
# using + operator
df['book_details'] = df['author'] + ', ' + df['book_name']

# using str.cat()
df['book_details'] = df['author'].str.cat(df['book_name'], sep = ', ')
```

|                         book_details |
|-------------------------------------:|
|J.R.R. Tolkien, The Lord of the Rings |
|J.D. Salinger, The Catcher in the Rye |
|                                  NaN |
|                                  NaN |
|                                  NaN |

#### Separator set to semi-colon ;
```python
# using + operator
df['book_details'] = df['author'] + '; ' + df['book_name']

# using str.cat()
df['book_details'] = df['author'].str.cat(df['book_name'], sep = '; ')
```

|                         book_details |
|-------------------------------------:|
|J.R.R. Tolkien; The Lord of the Rings |
|J.D. Salinger; The Catcher in the Rye |
|                                  NaN |
|                                  NaN |
|                                  NaN |

#### Separator set to space
```python
# using + operatorf
df['book_details'] = df['author'] + ' ' + df['book_name']

# using str.cat()
df['book_details'] = df['author'].str.cat(df['book_name'], sep = ' ')
```

|                         book_details |
|-------------------------------------:|
| J.R.R. Tolkien The Lord of the Rings |
| J.D. Salinger The Catcher in the Rye |
|                                  NaN |
|                                  NaN |
|                                  NaN |