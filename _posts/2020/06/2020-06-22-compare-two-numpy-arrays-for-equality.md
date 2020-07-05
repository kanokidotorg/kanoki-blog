---
title: "Compare two Numpy arrays for equality"
date: "2020-06-22"
categories: [ numpy, Python ]
tags: [ scikit-learn ]
---
In this post we will compare elements of two arrays for equality. This would be really helpful when you wanted to compare if two similar arrays coming out through two different processes are equal in shape and have equal elements

We will use some numpy built-in functions for checking the equality between the two arrays

Following three functions can be used for equality:

*   array_equality
*   allclose
*   array_equiv

## **Array Equality**

This function returns a boolean value True if the two arrays are of same shape and elements, otherwise false

```
a = np.arange(15).reshape(5,3)
b = np.arange(15).reshape(5,3)
c = np.arange(10).reshape(5,2)
a = b=  array([[ 0,  1,  2],
       [ 3,  4,  5],
       [ 6,  7,  8],
       [ 9, 10, 11],
       [12, 13, 14]])
```

```
a = b=  array([[ 0,  1,  2],
       [ 3,  4,  5],
       [ 6,  7,  8],
       [ 9, 10, 11],
       [12, 13, 14]])
c = array([[0, 1],
       [2, 3],
       [4, 5],
       [6, 7],
       [8, 9]])
```

```
c = array([[0, 1],
       [2, 3],
       [4, 5],
       [6, 7],
       [8, 9]])
```

Let’s check if a == b

```
np.array_equal(a, b)
```

True

Now check if a == c and b == c
```
np.array_equal(a, c)
```

False

## **allclose**

Returns True if two arrays are element-wise equal within a tolerance. The tolerance values are positive, typically very small numbers

The comparison of _a_rray a and _b_ uses standard broadcasting, which means that _a_ and _b_ need not have the same shape in order for `allclose(a, b)` to evaluate to True

```
a = [1,2]
b= [[1,2],[1,2]]
```

_Now array a can be broadcasted to create the same shape as b. so if we compare both these arrays a and b then it will give_ True

```
np.allclose(a, b)
```

True

## **array_equiv**

Returns True if input arrays are shape consistent and all elements equal

Shape consistent means they are either the same shape, or one input array can be broadcasted to create the same shape as the other one

So let’s compare the above two arrays using array_equiv

```
a = [1,2]
b= [[1,2],[1,2]]
```

```
np.array_equiv(a, b)
```

True

Let’s take another set of array where a cannot be broadcasted to b

```
a = [1, 2]
b = [[1, 2], [1, 3]]
```

Now if we compare these two arrays a and b then it will return False

np.array_equiv(a, b)

False

## **Conclusion**

We have seen three different numpy functions for comparing arrays. array_equal() function is useful to compare the shape and elements between two arrays.

allclose() and array_equiv() is used when all elements are equal and they are either the same shape, or one input array can be broadcasted to create the same shape as the other one

* * *