---
title: "Pandas how to use list of values to select rows from a dataframe"
date: 2023-03-06
categories: [matplotlib, python]
tags: [matplotlib, python]
last_modified_at: 2023-03-06
permalink: how-to-use-a-list-of-values-to-select-rows-from-a-dataframe
---

In this post we will see how to use a list of values to select rows from a pandas dataframe

We will follow these steps to select rows based on list of values:

1. Create a dataframe
2. `.isin()`, It checks whether each element in the DataFrame is contained in values

2. `.query()`, it let you query the columns of a DataFrame with a boolean expression
3. `.loc[]`, it is used to access a group of rows and columns by label(s) or a boolean array.
4. `numpy.in1d()`, It tests whether each element of a 1-D array is also present in a second array.



## Create Dataframe

First we will create a test dataframe with 4 columns: name, age, gender and city. 

Next we will see how to select the rows of this test dataframe by list of city values

```python
import pandas as pd

data = {'name': ['Alice', 'Bob', 
                 'Charlie', 'David', 'Eva'],
        'age': [25, 30, 35, 40, 45],
        'gender': ['female', 'male', 
                   'male', 'male', 'female'],
        'city': ['New York', 'Paris', 
                 'London', 'Sydney', 'Beijing']}
df = pd.DataFrame(data)
```

|      | name    | age  | gender | city     |
| ---- | ------- | ---- | ------ | -------- |
| 1    | Alice   | 25   | Female | New York |
| 2    | Bob     | 30   | Male   | Paris    |
| 3    | Charlie | 35   | Male   | London   |
| 4    | David   | 40   | Male   | Sydney   |
| 5    | Eva     | 45   | Female | Beijing  |

## Select Rows of dataframe by list of values:

#### Select rows using .isin()

We are using a `selected_cities` array as out list of values and want to filter the rows of dataframe where the city column contains these values

The `.isin()` method can be used to select rows where a column contains one or more of the specified values. Here's an example:

```python
selected_cities = ['London', 'Paris', 'Tokyo']

df_selected = df[df['city'].isin(selected_cities)]

df_selected
```

|      | name  | age  | gender | city     |
| ---- | ----- | ---- | ------ | -------- |
| 1    | Bob   | 30   | Male   | New York |
| 2    | Paige | 35   | Female | Dallas   |

Here, the `.isin()` method checks whether each element in the `city` column is contained in the `selected_cities` list. The resulting boolean Series is then used to select the rows where the condition is True.

#### Select rows using `.query()`

The `.query()` method can also be used to select rows based on a list of values. Here's an example:

```python
df_selected = df.query('city in @selected_cities')

print(df_selected)
```

Here, we use the `@` symbol to reference the `selected_cities` list inside the query string.

The output will be same as above

|      | name  | age  | gender | city     |
| ---- | ----- | ---- | ------ | -------- |
| 1    | Bob   | 30   | Male   | New York |
| 2    | Paige | 35   | Female | Dallas   |



#### Select rows using `.loc[]`

The `.loc[]` method can be used to select rows based on a list of values. Here's an example:

```python
df_selected = df.loc[df['city'].isin(selected_cities)]

print(df_selected)
```

Here, we use the boolean Series returned by the `.isin()` method as the index to select the rows where the condition is True.

The output will be same as above

#### Select rows using boolean indexing

We can also use boolean indexing to select rows based on a list of values. Here's an example:

```python
df_selected = (df[df['city']
               .apply(lambda x: x in selected_cities)])

print(df_selected)

```

Here, we use the `.apply()` method to apply a lambda function to each element in the `city` column, returning a boolean Series indicating whether the element is in the `selected_cities` list. The resulting boolean Series is then used to select the rows where the condition is True.

#### Select rows using  `numpy.in1d()`

Finally, we can use the `numpy.in1d()` function to select rows based on a list of values. Here's an example:

```python
import numpy as np

df_selected = df[np.in1d(df['city'], selected_cities)]

```

We use `np.in1d()` to create a boolean mask based on whether the values in the `city` column are in the `selected_cities` array. Finally, we use this mask to select the rows in the DataFrame that meet the condition.



