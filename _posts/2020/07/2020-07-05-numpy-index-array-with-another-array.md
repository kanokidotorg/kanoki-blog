---
title: "Index a Numpy Array by another Array"
date: "2020-07-05"
categories: [ numpy]
tags: [ numpy ]
---

In this post we will see different ways to Index a Numpy array using another array of index

Suppose we have a Matrix A:

```python
A = np.array([[0.32, 0.35, 0.88, 0.63, 1.  ],
              [0.23, 0.69, 0.98, 0.22, 0.96],
              [0.7 , 0.51, 0.09, 0.58, 0.19],
              [0.98, 0.42, 0.62, 0.94, 0.46],
              [0.48, 0.59, 0.17, 0.23, 0.98]])
```

and another Matrix B:

```python
B = np.array([[4, 0, 3, 2, 1],
              [3, 2, 4, 1, 0],
              [4, 3, 0, 2, 1],
              [4, 2, 0, 3, 1],
              [0, 3, 1, 2, 4]])
```

Now we wanted to arrange the matrix A based on the index in matrix B. The final matrix should look like this:

```python
array([[1.  , 0.32, 0.63, 0.88, 0.35],
       [0.22, 0.98, 0.96, 0.69, 0.23],
       [0.19, 0.58, 0.7 , 0.09, 0.51],
       [0.46, 0.62, 0.98, 0.94, 0.42],
       [0.48, 0.23, 0.59, 0.17, 0.98]])
```

One way is using nested loops i.e. by looping over the matrix A and index it with the values in B. However this method is highly inefficient and does not utilize the Numpy Vectorization and is extremely slow

Let's find out some efficient and performant way to reorder Matrix A using the indexes from Matrix B.

We're going to discuss about these three methods to reorder the values in Matrix A:

1. Linear Indexing using Numpy take() method
2. Numpy function take_along_axis()
3. Advanced Indexing

## Index Array by another Index Array using Numpy take()

numpy take() takes elements along an axis and returned array that has the same type as input Array.

We can also specify how out-of-bounds indices will behave by passing the mode parameter that takes following values raise(raises an error), wrap and clip

Another parameter indices takes the indices of the values to extract

A simple example to extract the elements from 1D array by another index array using numpy take()

```python
a = [6, 9, 5, 7, 3, 8]
indices = [0, 1, 4]
np.take(a, indices)
array([6, 9, 3])
```

Now let's jump to our original problem of reordering the Array A by Array B

First we will replace the index in B as a representation of Index for the whole Array. Currently the index is representing each row

```python
m,n = A.shape
B + n*np.arange(m)[:,None]
```

```python
array([[ 4,  0,  3,  2,  1],
       [ 8,  7,  9,  6,  5],
       [14, 13, 10, 12, 11],
       [19, 17, 15, 18, 16],
       [20, 23, 21, 22, 24]])
```

This returns Array B with index ranging from 0 to 24. Now we will use the take() method to get the Array A arranged by the index of new Array B

```python
m,n = A.shape
np.take(A,B + n*np.arange(m)[:,None])
```

**Result:**

```python
array([[1.  , 0.32, 0.63, 0.88, 0.35],
       [0.22, 0.98, 0.96, 0.69, 0.23],
       [0.19, 0.58, 0.7 , 0.09, 0.51],
       [0.46, 0.62, 0.98, 0.94, 0.42],
       [0.48, 0.23, 0.59, 0.17, 0.98]])
```

## Efficient way to Arrange 2D array from another Index Array using take_along_axis()

Numpy take_along_axis() method iterates over matching 1d slices oriented along the specified axis in the index and data arrays, and uses the former to look up values in the latter

It is as simple as this:

```python
np.take_along_axis(A,B,1)
```

A is the input Array A and Array B is Indices to take along each 1d slice of Array A and the last parameter axis is set to 1 i.e. the axis to take 1d slices along

This can be used for sorting along with numpy argsort as well. First sort any Array using Argsort and then this function can be used to get the sorted Array. However that is not in scope of this post.

## Advanced Indexing

There are two types of advanced indexing: integer and Boolean. Integer array indexing allows selection of arbitrary items in the array based on their *N*-dimensional index. Each integer array represents a number of indexes into that dimension

From each row, a specific element should be selected. The row index is just `[0, 1, 2, 3, 4]` and the column index specifies the element to choose for the corresponding row, here `[0, 1, 2, 3, 4]`. Using both together the task can be solved using advanced indexing

```python
A[np.arange(A.shape[0])[:,None],B]
```

## Conclusion

We have seen different methods to Index an Array A based on the indexes in Array B. All of these methods are fast and efficient but take_along_axis() is most elegant way to do this task. Other methods required Indexing Knowledge at an Advanced level.