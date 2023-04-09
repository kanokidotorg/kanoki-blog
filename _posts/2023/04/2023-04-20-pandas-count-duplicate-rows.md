---
title: "pandas count duplicate rows"
date: 2023-04-09
categories: [matplotlib, python]
tags: [matplotlib, python]
last_modified_at: 2023-04-09
permalink: pandas-count-duplicate-rows

---

DataFrames are a powerful tool for working with data in Python, and Pandas provides a number of ways to count duplicate rows in a DataFrame.

In this article, we’ll explore a few of the most common methods and highlight their advantages and disadvantages.

We will be using the following functions

-  `pivot_table()`
-  `groupby()`
-  `value_counts()`
-  `lambda`
-  `duplicated()`
-  `len()` with `drop_duplicates()`

We’ll also include some code examples to help you get started with counting duplicate rows in your own Pandas DataFrame.

In this blog post, we will discuss different ways to count duplicate rows in a pandas dataframe with a sample dataframe with 5 columns and 15 rows.

## Create Dataframe

Let's start by creating the test dataframe with duplicate rows

```python
# importing required libraries
import pandas as pd

# creating dataframe
df = pd.DataFrame(
    {
        'Name': ['John', 'Mary', 'John',
                 'John', 'John', 'Mary',
                 'Mary', 'John', 'John',
                 'John', 'Mary', 'John',
                 'John', 'Mary', 'John'],
        'Age': [30, 23, 30, 30, 30, 23, 23,
                30, 30, 30, 23, 30, 30, 23,
                30],
        'Gender': ['M', 'F', 'M', 'M', 'M',
                   'F', 'F', 'M', 'M', 'M',
                   'F', 'M', 'M', 'F', 'M'],
        'Job': ['Doctor', 'Teacher', 'Student',
                'Doctor', 'Student', 'Teacher',
                'Teacher', 'Doctor', 'Student',
                'Doctor', 'Teacher', 'Student',
                'Doctor', 'Teacher', 'Student'],
        'Salary': [100000, 50000, 30000,
                   100000, 30000, 50000,
                   50000, 100000, 30000,
                   100000, 50000, 30000,
                   100000, 50000, 30000]
    })

# displaying the dataframe
df
```

**Out:**

|    | Name | Age | Gender |     Job | Salary |
|---:|-----:|----:|-------:|--------:|-------:|
|  0 | John |  30 |      M |  Doctor | 100000 |
|  1 | Mary |  23 |      F | Teacher |  50000 |
|  2 | John |  30 |      M | Student |  30000 |
|  3 | John |  30 |      M |  Doctor | 100000 |
|  4 | John |  30 |      M | Student |  30000 |
|  5 | Mary |  23 |      F | Teacher |  50000 |
|  6 | Mary |  23 |      F | Teacher |  50000 |
|  7 | John |  30 |      M |  Doctor | 100000 |
|  8 | John |  30 |      M | Student |  30000 |
|  9 | John |  30 |      M |  Doctor | 100000 |
| 10 | Mary |  23 |      F | Teacher |  50000 |
| 11 | John |  30 |      M | Student |  30000 |
| 12 | John |  30 |      M |  Doctor | 100000 |
| 13 | Mary |  23 |      F | Teacher |  50000 |
| 14 | John |  30 |      M | Student |  30000 |

Now that we have our DataFrame created, let’s look at some of the ways we can count duplicate rows.

## Count Duplicates

### pivot_table

We'll discuss is the `pivot_table` function in this section.

This function takes in the DataFrame and the columns you want to group by, and then counts the number of duplicates in each group.

To use the `pivot_table` function, we'll pass in the DataFrame, the columns we want to group by, and the values we want to count.

The `pivot_table` function returns a DataFrame with the counts of duplicates for each group.

#### pivot_table - index value

```python
count_dup = df.pivot_table(index = ['Name'],
                           aggfunc = 'size')
```

| Name         |    |
|--------------|----|
| John         | 10 |
| Mary         | 5  |

#### pivot_table - multiple index

```python
count_dup = df.pivot_table(index = ['Name', 'Job'],
                           aggfunc = 'size')
```

| Name | Job     |      |
| ---- | ------- | ---- |
| John | Doctor  | 5    |
| John | Student | 5    |
| Mary | Teacher | 5    |

#### pivot_table - multiple column

```python
count_dup = df.pivot_table(columns = ['Name', 'Age',
                                      'Gender', 'Job', 'Salary'],
                           aggfunc = 'size')
```

| Name | Age | Gender | Job     | Salary |   |
|------|-----|--------|---------|--------|---|
| John | 30  | M      | Doctor  | 100000 | 5 |
|      |     |        | Student | 30000  | 5 |
| Mary | 23  | F      | Teacher | 50000  | 5 |

### groupby

We'll discuss is the `groupby` function in this section.

This function takes in the DataFrame and the columns you want to group by and returns a DataFrame with the counts of duplicates in each group.

To use the `groupby` function, we’ll pass in the DataFrame, the columns we want to group by, and the values we want to count.

The `groupby` function returns a DataFrame with the counts of duplicates for each group.

#### groupby - column

```python
count_dup = df.groupby('Name').size()
```

| Name |    |
|------|----|
| John | 10 |
| Mary | 5  |

#### groupby - multiple column

```python
count_dup = df.groupby(['Name', 'Age', 'Gender', 'Job', 'Salary']).size()
```

| Name | Age | Gender | Job     | Salary |   |
|------|-----|--------|---------|--------|---|
| John | 30  | M      | Doctor  | 100000 | 5 |
|      |     |        | Student | 30000  | 5 |
| Mary | 23  | F      | Teacher | 50000  | 5 |

#### groupby - reset_index

```python
count_dup = (
              df.groupby(df.columns.tolist())
              .size()
              .reset_index()
              .rename(columns = {0:'count'})
            )
```

|   | Name | Age | Gender |     Job | Salary | count |
|--:|-----:|----:|-------:|--------:|-------:|------:|
| 0 | John |  30 |      M |  Doctor | 100000 |     5 |
| 1 | John |  30 |      M | Student |  30000 |     5 |
| 2 | Mary |  23 |      F | Teacher |  50000 |     5 |

#### groupby - reset_index by column

```python
count_dup = (
             df.groupby('Job')
             .size()
             .reset_index()
             .rename(columns = {0:'count'})
            )
```

|   |     Job | count |
|--:|--------:|------:|
| 0 |  Doctor |     5 |
| 1 | Student |     5 |
| 2 | Teacher |     5 |

### value_counts

We'll discuss is the `value_counts` function in this section.

This function takes in the DataFrame and the columns you want to count and returns a Series with the counts of unique values in each column.

To use the `value_counts` function, we’ll pass in the DataFrame and the columns we want to count.

#### value_counts - column as index

```python
count_dup = df['Name'].value_counts().reset_index(name = 'count')
```

|   | index | count |
|--:|------:|------:|
| 0 |  John |    10 |
| 1 |  Mary |     5 |

#### value_counts - column as value

```python
count_dup = df['Job'].value_counts()
```

```python
counts = df[['Job']].apply(pd.value_counts)
```

Both give the same output.

| Doctor                  | 5 |
|-------------------------|---|
| Teacher                 | 5 |
| Student                 | 5 |

#### value_counts - multiple column

```python
count_dup = df[['Name', 'Age']].value_counts().reset_index(name = 'count')
```

|   | Name | Age | count |
|--:|-----:|----:|------:|
| 0 | John |  30 |    10 |
| 1 | Mary |  23 |     5 |

#### value_counts - all columns

```python
count_dup = df.value_counts().reset_index(name = 'count')
```

|   | Name | Age | Gender |     Job | Salary | count |
|--:|-----:|----:|-------:|--------:|-------:|------:|
| 0 | John |  30 |      M |  Doctor | 100000 |     5 |
| 1 | John |  30 |      M | Student |  30000 |     5 |
| 2 | Mary |  23 |      F | Teacher |  50000 |     5 |

### lambda

We'll discuss is the `lambda` function in this section.

This function takes in the DataFrame, the columns you want to group by, and then returns the number of unique rows in each group.

To use the lambda function, we’ll pass in the DataFrame, the columns we want to group by, and then use the len() function to count the number of unique rows.

The lambda function returns a Series with the number of unique rows in each group.

#### lambda - column

```python
count_dup = df.groupby('Name').apply(lambda x: len(x.dropna()))

count_dup = count_dup.to_frame(name = 'count').reset_index()
```

|   | Name | count |
|--:|-----:|------:|
| 0 | John |    10 |
| 1 | Mary |     5 |

#### lambda - multiple column

```python
count_dup = df.groupby(['Name', 'Job']).apply(lambda x: len(x.dropna()))

count_dup = count_dup.to_frame(name = 'count').reset_index()
```

|   | Name |     Job | count |
|--:|-----:|--------:|------:|
| 0 | John |  Doctor |     5 |
| 1 | John | Student |     5 |
| 2 | Mary | Teacher |     5 |

#### lambda - total count of duplicates

This counts duplicates across the dataframe.
It returns the value and count of how many times the duplicates appears.

```python
count_dup = df.apply(lambda x: ' '
                     .join([f'[value = {i}, count = {v}]'
                            for i, v in x.value_counts().iteritems() if v > 1])
                    )
```

| Name          | [value = John, count = 10] [value = Mary, coun... |
|---------------|---------------------------------------------------|
| Age           | [value = 30, count = 10] [value = 23, count = 5]  |
| Gender        | [value = M, count = 10] [value = F, count = 5]    |
| Job           | [value = Doctor, count = 5] [value = Teacher, ... |
| Salary        | [value = 100000, count = 5] [value = 50000, co... |

#### lambda - total count of duplicates

This gives similar result as to the above code but here we import a library to show the output. It shows how many times the duplicate values appear.

```python
from collections import Counter

def count(x):
    return {v:c for v, c in x.items() if c > 1}

count_dup = df.apply(lambda x : count(Counter(x)))
```

|   Name |                   {'John': 10, 'Mary': 5} |
|-------:|------------------------------------------:|
|    Age |                           {30: 10, 23: 5} |
| Gender |                         {'M': 10, 'F': 5} |
|    Job | {'Doctor': 5, 'Teacher': 5, 'Student': 5} |
| Salary |           {100000: 5, 50000: 5, 30000: 5} |

### duplicated

We'll discuss is the `duplicated` function in this section.

This function takes in the DataFrame and returns a Boolean array of the duplicate rows.

To use the `duplicated` function, we’ll pass in the DataFrame and check for duplicates.

By default, for each set of duplicated values, the first occurrence is set on False and all others on True.

#### duplicated - sum

```python
count_dup = df.duplicated().sum()

count_dup.head()
```
This outputs the total number of duplicate rows in the dataframe.

**Out:** 12

#### duplicated with value_counts

```python
count_dup = df.duplicated().value_counts()
```

Here, **True** are the duplicate rows, and **False** are the unique rows i.e. non-duplicate.

| True  | 12 |
|-------|----|
| False | 3  |

### len with drop_duplicates

We'll discuss is the `len` function with `drop_duplicates` function to check the repeating rows in this section.

We will use the `len` function to get the count of duplicates on a specific row or the entire DataFrame.

These functions return the length or count of the total number of duplicate single rows in a dataframe.

#### count of total non-duplicate single rows

```python
len(df.drop_duplicates())
```

**Out:** 3

#### count of total duplicate rows in a column

```python
count_dup = len(df['Job']) - len(df['Job'].drop_duplicates())
```

**Out:** 12

#### count of total duplicate rows in the dataframe

```python
count_dup = len(df) - len(df.drop_duplicates())
```

**Out:** 12

So, there are a number of ways to count duplicate rows in a Pandas DataFrame.

Each of these methods has its own advantages and disadvantages, so it’s important to understand the different use cases for each one.

With this knowledge, you should be able to count duplicate rows in your own Pandas DataFrames with ease.