---
title: "Pandas iterate over rows and update or Update dataframe row values where certain condition is met"
date: 2022-10-10
categories: [pandas]
tags: [pandas]
last_modified_at: 2022-10-10
permalink: pandas-iterate-over-rows-and-update-or-update-dataframe-row-values-with-condition
---

We want to iterate over the rows of a dataframe and update the values based on condition. There are three different pandas function available that let you iterate through the dataframe rows and columns of a dataframe.

- Dataframe.iterrows()
- Dataframe.itertuples()
- Dataframe.items()

Before we dive into these three functions, Let me make it very clear that iterating through a dataframe rows and columns should be the last resort since it's slow and not worth it. we can achieve anything using vectorization, loc and apply function. 

You should only use these functions when you are exhausted of all the solutions for your problem

## <span style="color:blue">Pandas update dataframe rows with condition </span>

Let's create a dataframe of students with their marks and grade

```python
df = pd.DataFrame({'name': ['Alex', 'Harry', 'Brenden', 'Alice', 'Ronald', 'Justin'], 
                   'marks': [50, 42, 35, 68, 47, 62],
                   'grade': ['B', 'B', 'B', 'A+', 'B+','A+']})
df
```

|      | name    | marks | Score |
| ---- | ------- | ----- | ----- |
| 0    | Alex    | 50    | B+    |
| 1    | Harry   | 42    | B     |
| 2    | Brenden | 35    | B-    |
| 3    | Alice   | 68    | A+    |
| 4    | Ronald  | 47    | B+    |
| 5    | Justin  | 62    | A+    |

**Condition:** We want to upgrade the grade of all students to A+ who has scored greater than 40 and less than 50

### <span style="color:brown">Update the dataframe values using Vectorization</span>

Let's update the value of dataframe using vectorization. We need to first create a function that supports vectorization.

We've used [numpy.where](https://kanoki.org/2020/01/03/how-to-work-with-numpy-where/) to get the index of all such rows that meets the condition and returns the new grade otherwise it returns the existing value. The return value of [numpy.where](https://kanoki.org/2020/01/03/how-to-work-with-numpy-where/) condition is stored in the column grade.

Numpy where supports vectorization and provides users with a wide variety of functions capable of performing operations on arrays of data. Its use of **vectorization** makes these custom functions incredibly fast.

```python
def update_color_col(x):
    x.grade = np.where(((x.score>40) & (x.score<50)),'A+',x.grade)
```

Let's call this function and pass the above created dataframe

```python
update_color_col(df)
df
```

|      | name    | marks | Score |
| ---- | ------- | ----- | ----- |
| 0    | Alex    | 50    | B+    |
| 1    | Harry   | 42    | A+    |
| 2    | Brenden | 35    | B-    |
| 3    | Alice   | 68    | A+    |
| 4    | Ronald  | 47    | A+    |
| 5    | Justin  | 62    | A+    |

Let's timeit this vectorization process

```python
%%timeit
update_color_col(df)
```

```python
276 µs ± 1.87 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```



### <span style="color:brown">Update the dataframe values using Apply</span>

We are updating the grade column using the lambda function inside apply, The axis is set to 1, to apply this function accross the dataframe columns. 

```python
df["grade"] = df.apply(lambda x: 'A+' if ((x['score']>40)&(x['score']<50)) else x["grade"], axis=1)
```

|      | name    | marks | Score |
| ---- | ------- | ----- | ----- |
| 0    | Alex    | 50    | B+    |
| 1    | Harry   | 42    | A+    |
| 2    | Brenden | 35    | B-    |
| 3    | Alice   | 68    | A+    |
| 4    | Ronald  | 47    | A+    |
| 5    | Justin  | 62    | A+    |

```python
%%timeit
df['color'] = df.apply(update_color_col,axis=1)
```

```python
707 µs ± 21.1 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```

We can see that the apply method is slower than the Vectorization approach above, The Vectorization approach is 10x faster than using apply. 

I think if we have a bigger dataframe with more number of rows then you can see an incredible result with the Vectorization approach

### <span style="color:brown">Update the dataframe values using loc</span>

We have to locate the row value first which meets the condtion using loc and then set the column value with the new values.

```python
df.loc[((df['score'] > 40)&(df['score'] < 50)), ['grade'] ] = 'A+'
df
```



|      | name    | marks | Score |
| ---- | ------- | ----- | ----- |
| 0    | Alex    | 50    | B+    |
| 1    | Harry   | 42    | A+    |
| 2    | Brenden | 35    | B-    |
| 3    | Alice   | 68    | A+    |
| 4    | Ronald  | 47    | A+    |
| 5    | Justin  | 62    | A+    |

Let's timeit to check which is fastest out of these 3 approaches to update the dataframe value without actually iterationg over the rows.

```python
%%timeit
df.loc[((df['score'] > 40)&(df['score'] < 50)), ['grade'] ] = 'A+'
```

```python
662 µs ± 16.6 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```

<br>

Alright! We can see that Vectorization is the fastest approach out of all the three to update the dataframe, the runner-up is loc and the last in list is apply function.

## <span style="color:blue">Pandas iterate over rows of dataframe and update</span>

Now we will see the pandas functions that can be used to iterate the rows and columns of a dataframe

We will use the same above dataframe(df) and the same condition to upgrade the grade of students where row condition is met, However this time we will iterate through the rows and columns of the dataframe to achieve this.

### <span style="color:brown">Dataframe.iterrows()</span>

It iterates over Dataframe rows as (Index, Series) pairs and returns a series for each row and itt does **not** preserve dtypes across the rows

Let's see the index of all the row values returned by itertuples, The index is the column label of the dataframe

```python
for idx, row in df.iterrows():
    print(row.index)
```

Out:

```python
Index(['name', 'score', 'grade'], dtype='object')
Index(['name', 'score', 'grade'], dtype='object')
Index(['name', 'score', 'grade'], dtype='object')
Index(['name', 'score', 'grade'], dtype='object')
Index(['name', 'score', 'grade'], dtype='object')
Index(['name', 'score', 'grade'], dtype='object')
```

Let's check the values of each of these rows returned by itertuples, the value for each row is returned as an array

```python
for idx, row in df.iterrows():
    print(row.values)
```

Out:

```python
['Alex' 50 'B+']
['Harry' 42 'A+']
['Brenden' 35 'B-']
['Alice' 68 'A+']
['Ronald' 47 'A+']
['Justin' 62 'A+']
```

We can access the column value of each row to check the condition and use dataframe.at() function to set the value of columns which meet our condition.

You can read more about updating the cell value in a dataframe [here](https://kanoki.org/2019/04/12/pandas-how-to-get-a-cell-value-and-update-it/).

```python
for idx, row in df.iterrows():
    if (row.score > 40) & (row.score < 50):
        df.at[idx,'grade'] = 'A+'
df      
```

Out:

|      | name    | marks | Score |
| ---- | ------- | ----- | ----- |
| 0    | Alex    | 50    | B+    |
| 1    | Harry   | 42    | A+    |
| 2    | Brenden | 35    | B-    |
| 3    | Alice   | 68    | A+    |
| 4    | Ronald  | 47    | A+    |
| 5    | Justin  | 62    | A+    |

### <span style="color:brown">Dataframe.itertuples()</span>

It Iterates over DataFrame rows as namedtuple and returns an object to iterate over namedtuples for each row in the DataFrame with the first field  being the index and following fields being the column values

If I just iterate through the return value of Itertuples

```python
for row in df.itertuples():
    print(row)
```

Out:

 You can see here, the Pandas is the name of returned named tuple and the other values are the column value of each row

```python
Pandas(Index=0, name='Alex', score=50, grade='B+')
Pandas(Index=1, name='Harry', score=42, grade='A+')
Pandas(Index=2, name='Brenden', score=35, grade='B-')
Pandas(Index=3, name='Alice', score=68, grade='A+')
Pandas(Index=4, name='Ronald', score=47, grade='A+')
Pandas(Index=5, name='Justin', score=62, grade='A+')
```

We could access the column using the dot notation and check the condition and use dataframe.at() to update the values of the column

```python
for row in df.itertuples():
    if (row.score > 40) & (row.score < 50):
        df.at[row.Index,'grade'] = 'A+'
df
```

Out:

|      | name    | marks | Score |
| ---- | ------- | ----- | ----- |
| 0    | Alex    | 50    | B+    |
| 1    | Harry   | 42    | A+    |
| 2    | Brenden | 35    | B-    |
| 3    | Alice   | 68    | A+    |
| 4    | Ronald  | 47    | A+    |
| 5    | Justin  | 62    | A+    |



### <span style="color:brown">Dataframe.items()</span>

It iterates over column name, Series pairs and returns a tuple with the column name and the content as a Series.

Let's check the label of each item in the iterable. 

```python
for label, content in df.items():
    print(content.name)
    # or
    print(label)
```

It throws out the column labels of the dataframe

Out:

```python
name
score
grade
```

And, each column is stored as Series:

```python
for label, content in df.items():
    print(content.values)
```

Out:

```python
['Alex' 'Harry' 'Brenden' 'Alice' 'Ronald' 'Justin']
[50 42 35 68 47 62]
['B+' 'A+' 'B-' 'A+' 'A+' 'A+']
```

We will get the index of each row which meets the condition and then update the value of the grade column of these rows using dataframe.at() function

```python
for label, content in df.items():
    if content.name == 'score':
        for idx in np.where((content>40)&(content<50))[0]:
            df.at[idx,'grade'] = 'A+'   
df            
```



|      | name    | marks | Score |
| ---- | ------- | ----- | ----- |
| 0    | Alex    | 50    | B+    |
| 1    | Harry   | 42    | A+    |
| 2    | Brenden | 35    | B-    |
| 3    | Alice   | 68    | A+    |
| 4    | Ronald  | 47    | A+    |
| 5    | Justin  | 62    | A+    |

