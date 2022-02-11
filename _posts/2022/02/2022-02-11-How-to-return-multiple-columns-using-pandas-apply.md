---

title: "How to return multiple columns using pandas apply"
date: "2022-02-11"
categories: [ python, pandas]
tags: [ python, pandas]

---

In this article we are going to compare the performance of two approaches i.e. Apply method and Vectorization which can be used to apply a function on dataframe and return multiple new columns

we will be following the below steps to create those multiple columns for a dataframe using a custom function:

1. Create a dataframe with 10K rows and two columns name and size(bytes) respectively
2. Build a function to convert the size to Kilobytes(kb), Megabytes(MB) and Gigabytes(GB) 
2. Use *pandas.Series.apply()* to apply the function created in Step#2 above to create three new columns size_kb, size_mb and size_gb respectively
3. Use *Vectorization* approach to apply the function created in Step#2 above to create three new columns size_kb, size_mb and size_gb respectively
4. Compare the performance of *pandas.Series.apply()* and *Vectorization* to apply a function on dataframe and get three new columns
5. Why pandas.apply() is slower and one should always use Vectorization for working with large dataset

**Let's get started**

## Create a dataframe

Let's create a dataframe with two columns, name and size. we will use random strings and integers to create these columns for a dataframe of shape (10000, 2) 

```python
import pandas as pd
import numpy as np
import locale
import random, string

N = 10000
np.random.seed(0)
rand_name_lst=[''.join(random.choices(string.ascii_uppercase + string.digits, k=5)) \
                            for i in range(N)]
rand_int_lst = np.random.randint(912131,18293293,size=N)
df = pd.DataFrame({'name': rand_name_lst , 'size': rand_int_lst})

df.sample(5)
```

Let's visualize the sample of size 5 from the dataframe

| | name| size|
| ---- | --- | --- |
| 7463|L15I5| 9237935 |
| 6722|QX4AM | 3127235 |
| 6182|TTTJ2 | 6069830
| 31|CTB7Y | 15951978 |	
| 1180|X7JCG | 8556300 |


## Create a function to convert the size into KB, MB and GB

This function takes an input param(x) and returns a Pandas series object of three values i.e. converted size in KB, MB and GB

```python
def convert_size(x):
    kb = round((x / 1024.0),2)
    mb = round((x / (1024.0 ** 2)),2)
    gb = round((x / (1024 **3)),2)

    return pd.Series([kb, mb, gb])
```

## Use Pandas Apply function to return multiple columns

First, we will use *pandas.series.apply()* to apply the convert_size() function on the dataframe column "size" to convert the size into KB, MB and GB by creating three new columns i.e. `size_kb`, `size_mb` and `size_gb` respectively

We are using magic function to know about the performance of *pandas.series.apply()* function which is used to create three new columns on a dataframe of 10K rows

```python
%%timeit
df[['size_kb','size_mb','size_gb']]=df['size'].apply(convert_size)
```

**Out:** 1.24 s ± 60.9 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

It took around **1.24 seconds** for apply function to return these multiple columns 

Let's check the sample of dataframe and verify the three newly created columns (size_kb, size_mb and size_gb) 

```python
%%timeit
df.sample(5)
```

| | name| size|size_kb| size_mb| size_gb|
| ---- | --- | --- | --- | --- | --- |
| 7463| AFV78 | 2216376 | 2164.43 | 2.11 | 0.00|
| 6722| LYUDP | 9639260 | 9413.34 | 9.19 | 0.01|
| 6182| RHNAZ | 13829302 | 13505.18 | 13.19 | 0.01|
| 31| IF6CG | 9906803 |	 9674.61 | 9.45 | 0.01|
| 1180| XBCNC | 8793505 | 8587.41 | 8.39 | 0.01|


## Use Vectorization to return multiple columns


Let's create these 3 new columns using the Vectorization approach but What is Vectorization? 

Vectorization lets you perform an operation on arrays without any iterations, loops over the dataframe rows, whereas pandas apply() method uses for loop under the hood. In a Vectorize approach a function takes a sequence or series of object and evaluates the custom function on each element of the input series

You can see here, we are creating those 3 new columns `size_kb`, `size_mb` and `size_gb` using vectorized approach by passing size column a pd.Series object to the `convert_size()` function defined above

**Note:** Please re-create the dataframe before running the below code to create new columns using Vectorization

```python
%%timeit
df['size_kb'],df['size_mb'],df['size_gb']=convert_size(df['size'])
```

**Out:** 764 µs ± 76.6 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

It took 764 micro-seconds to create those 3 new columns on a dataframe of 10K rows

## Pandas Apply vs Vectorization

So you have seen it took 1.24 seconds using apply function to create multiple columns whereas using the Vectorization approach it took only 764 micro seconds. which is a performance gain of 1000x. Isn't it Amazing?

Pandas apply() are slow and under the hood it iterates over the rows of dataframe, whereas Vectorization is a modern way to evaluate a function on each element of the array simultaneously

## Conclusion:
 - *pandas.series.apply()* can be used to return multiple columns by applying a custom function that can return a pandas series object
 - Vectorization is a faster approach to create multiple columns by applying a function element-wise to the dataframe array
 - With a Vectorization approach we can gain a performance of 1000x on a dataframe with 10K rows
