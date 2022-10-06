---
title: "Numpy get ith column and specific column and row data from an array"
date: "2022-10-06"
categories: [python, numpy]
tags: [python, numpt]
last_modified_at: 2022-10-06
permalink: numpy-get-ith-column-and-specific-column-row-data-from-array
---

We want to select specific column and rows in a numpy array

In this post we will see the slicing and indexing of numpy array for the following scenarios:

- get column by index in a numpy array
- get multiple columns by index
- get specific rows and their columns
- get last column

Beside this, we will also see how to use transpose and ellipsis to get the specific row and column values from an ndarray

Ndarrays can be indexed using the standard python syntax, There are different kinds of indexing available such as basic indexing, advanced indexing and field access.

## <span style="color:blue">get specific column by index</span>

Let's create a numpy array 

```python
a=np.random.randint(1, 105, size=(5, 4))
a
```

```python
array([[76, 43, 27, 50],
       [10, 34, 40,  3],
       [11, 33, 46, 68],
       [27, 14, 30, 85],
       [32, 92, 73, 39]])
```

To get all the rows of 2nd column of the array

```python
a[:,2]
```

```python
array([27, 40, 46, 30, 73])
```

We could also use Ellipsis to get the same result

[`Ellipsis`](https://docs.python.org/3/library/constants.html#Ellipsis) expands to the number of`:`objects needed for the selection tuple to index all dimensions.

```python
a[...,2]
```

```python
array([27, 40, 46, 30, 73])
```



## <span style="color:blue">get multiple columns by index</span>

To get multiple columns in a numpy array, we will pass that as a list, shown below

We want the 2nd and 3rd column of the array a.

```python
a[:,[2,3]]
```

```python
array([[27, 50],
       [40,  3],
       [46, 68],
       [30, 85],
       [73, 39]])
```

## <span style="color:blue">get rows and columns by index</span>

In this case we want to get all the values upto 3rd row for the 2nd and 3rd column only

```python
a[:3,[2,3]]
```

```python
array([[27, 50],
       [40,  3],
       [46, 68]])
```

## <span style="color:blue">get last column</span>

We want the last column of the array, we could use negative indices for indexing from the end of the array

```python
a[:,-1]
```

```python
array([50,  3, 68, 85, 39])
```

The output array is 1D of shape 5. We could also change the shape of the output array by using newaxis.

Each newaxis object in the selection tuple serves to expand the dimensions of the resulting selection by one unit-length dimension. 

The added dimension is the position of the newaxis object in the selection tuple. newaxis is an alias for `None`, and `None` can be used in place of this with the same result. 

We want a 2D array as output instead of an 1D 

```python
a[np.newaxis,:,-1]

# or

a[None,:,-1]
```

Out:

```python
array([[50,  3, 68, 85, 39]])
```

Let's find out the shape of this new output

```python
a[np.newaxis,:,-1].shape
```

Out:

```python
(1,5)
```



## <span style="color:blue">get first n rows of 2nd column</span>

In this case, we want all the values of 2nd column upto 3rd row

```python
a[:3,2]
```

```python
array([27, 40, 46])
```

Alternatively,we could also use Transpose

```python
a.T[2][:3]
```

```python
array([27, 40, 46])
```

