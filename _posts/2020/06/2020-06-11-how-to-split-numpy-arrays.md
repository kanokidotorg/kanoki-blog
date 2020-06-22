---
title: "How to split Numpy Arrays"
date: "2020-06-11"
categories: [ scikit-learn ]
tags: [ scikit-learn ]
---

In this post we will see how to split a 2D numpy array using split, array_split , hsplit, vsplit and dsplit.

These split functions let you partition the array in different shape and size and returns list of Subarrays

*   **split()**: Split an array into multiple sub-arrays of equal size
*   **array_split()**: It Split an array into multiple sub-arrays of equal or near-equal size. Does not raise an exception if an equal division cannot be made.
*   **hsplit()**: Splits an array into multiple sub-arrays horizontally (column-wise).
*   **vsplit()**: It Split array into multiple sub-arrays vertically (row wise).
*   **dsplit()**: Splits an array into multiple sub-arrays along the 3rd axis (depth).

## **Split**

This method takes takes following three arguments and returns list of Sub arrays. The array will be divided into N equal arrays along axis. If such a split is not possible, an error is raised

**array**: An ndarray object for partition
**indices_or_sections**: int or 1D array
**axis**: the axis along which to split

### **Split 1D Array**

Let’s create a 1D array first

```
import numpy as np
x = np.arange(10)
x
```
```
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

Let’s split the above 1-D array into 5 equal part using numpy split() method

```
np.split(x, 5)
```

```
[array([0, 1]), array([2, 3]), array([4, 5]), array([6, 7]), array([8, 9])]
```

Next we will pass a 1-D array for partition of x i.e. list of indices along which Array to be divided into sub-arrays

```
np.split(x, [4, 6, 7, 9])
```

```
[array([0, 1, 2, 3]), array([4, 5]), array([6]), array([7, 8]), array([9])]
```

We passed a 1D indices list [4, 6, 7, 9] for partition so x is divided into following subarrays:

1st Partition: Index 0 to 3 because 1st element in 1D indices list is 4
_array([0, 1, 2, 3])_

2nd Partition: Index 4 to 5 between 1st and 2nd element of 1D indices list
_array([4,5])_

3rd Partition: Index 6 to 6 between 2nd and 3rd element of 1D indices list
_array([6])_

4th Partition: Index 7 to 8 between 3rd and 4th element of 1D indices list
_array([7,8])_

5th Partition: Index 9 to last between 4th element and last index of indices list
_array([9])_

### **Split 2D Array**

Let’s create a 2D array first

```
x = np.arange(16).reshape(4,4)
x
```

```
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
```

We will split the above 2D array into Sub array using indices list of [1,3]

```
np.split(x, [1,3])
```

```
[array([[0, 1, 2, 3]]), array([[ 4,  5,  6,  7],
        [ 8,  9, 10, 11]]), array([[12, 13, 14, 15]])]
```

1st Partition: Index 0 along axis = 0 becasue 1st element in indices list is 1
_array([0,1,2,3])_

2nd Partition: Index 1 to 2 along axis = 0 between 1st and 3rd element in indices list
_array([0,1,2,3])_

3rd Partition: Index 3 to 3 along axis = 0 between 3rd element of indices list and last index of 2D array i.e. 3
_array([0,1,2,3])_

## **Array_Split**

This method is same as split above and only difference is that it allows unequal partition also

Here is a 2D array that we will split into unequal subarrays

```
x = np.arange(16).reshape(4,4)
x
```

```
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])
```

Let’s split into 3 so equal partitioning is not possible so let’s see how it works

```
np.array_split(x,3)
[array([[0, 1, 2, 3],[4, 5, 6, 7]]),
array([[ 8,  9, 10, 11]]),
array([[12, 13, 14, 15]])]
```

if l is length of Array and n is the expected no. of parition of the array then

it returns l % n sub-arrays of size l//n + 1 which is 1 subarray of size 2 and the first subbarray above is array([[0, 1, 2, 3],[4, 5, 6, 7]])

Other two subarrays will be of size l//n i.e. 1 and that gives these two sub arrays array([[ 8, 9, 10, 11]]), array([[12, 13, 14, 15]])

## **Split Array by column using hsplit**

Splits an array into multiple sub-arrays horizontally(Column wise). hsplit is equivalent to split with axis=1

The array is always split along the second axis regardless of the array dimension

```
x = np.arange(20).reshape(5, 4)
x
```

```
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15],
       [16, 17, 18, 19]])
```

Next we will split the above array at index 2

```
np.hsplit(x,2)
```
```
[array([[ 0,  1],
        [ 4,  5],
        [ 8,  9],
        [12, 13],
        [16, 17]]),
array([[ 2,  3],
        [ 6,  7],
        [10, 11],
        [14, 15],
        [18, 19]])]
```

This splits a 5×4 array into two 5X2 arrays

Let’s give a list of indices to split

```
np.hsplit(x, [3,4])
```

```
[array([[ 0,  1,  2],
        [ 4,  5,  6],
        [ 8,  9, 10],
        [12, 13, 14],
        [16, 17, 18]]),
array([[ 3],
        [ 7],
        [11],
        [15],
        [19]]),
array([], shape=(5, 0), dtype=int32)]
```

It splits into 3 subarrays. First one is of size 5X3 and second of size 5X1 and third is a Blank array

We already discussed this that first it splits between 0 and 2 since the first index for split is 3 and Secondly between indices 3 and 4 since second element is 5 and at Last anything beyond index 5

## **Split array by rows using vsplit**

Splits an array into multiple sub-arrays vertically (row-wise)

`vsplit` is equivalent to `split` with _axis=0_ (default), the array is always split along the first axis regardless of the array dimension

This works exactly same as split function discussed above

## **Split Arrays along Third axis i.e. axis = 2 using dsplit**

Split array into multiple sub-arrays along the 3rd axis (depth)

dsplit is equivalent to split with axis=2\. The array is always split along the third axis provided the array dimension is greater than or equal to 3

```
x = np.arange(24).reshape(2, 2, 6)
x
```

```
array([[[ 0,  1,  2,  3,  4,  5],
        [ 6,  7,  8,  9, 10, 11]],

       [[12, 13, 14, 15, 16, 17],
        [18, 19, 20, 21, 22, 23]]])
```

Let’s split this array along the following list of indices [2,3]

```
np.dsplit(x, np.array([2, 3]))
```

```
[array([[[ 0,  1],
         [ 6,  7]],

        [[12, 13],
         [18, 19]]]),
array([[[ 2],
         [ 8]],

        [[14],
         [20]]]),
array([[[ 3,  4,  5],
         [ 9, 10, 11]],

        [[15, 16, 17],
         [21, 22, 23]]])]
```

So it splits a 2x2x6 matrix into three subarrays of size: 2x2x2, 2x2x1 and 2x2x3

## **Split Arrays at Values**

if we want to split an array at a specific value then just use np.where to find the index of that value and pass that as a parameter for splitting

```
import numpy as np
x = np.arange(16).reshape(8,2)
x
```
```
import numpy as np
x = np.arange(16).reshape(8,2)
x
array([[ 0,  1],
       [ 2,  3],
       [ 4,  5],
       [ 6,  7],
       [ 8,  9],
       [10, 11],
       [12, 13],
       [14, 15]])
```

Let’s split this array at value 6\. So we will first determine the index of this element in the array using np.where() method

```
np.array_split(A, np.where(A[:, 0]== 6)[0][0])
```
```
[array([[0, 1],
        [2, 3],
        [4, 5]]),
array([[ 6,  7],
        [ 8,  9],
        [10, 11]]),
array([[12, 13],
        [14, 15]])]
```

So it splits a 8×2 Matrix into 3 unequal Sub Arrays of following sizes: 3×2, 3×2 and 2×2

## **Conclusion**

Here are the points to summarize our learning about array splits using numpy

*   Numpy Split() function splits an array into multiple sub arrays
*   Either an interger or list of indices can be passed for splitting
*   Split() function works along either axis 0 or 1
*   array_split() function splits the array into unequal size subarrays unlike split() function
*   hsplit() function can be used to split an array by column. it is same as split() function with axis = 1
*   vsplit() function is same as split() function with axis = 0 i.e. default
*   dsplit() Splits array into multiple sub-arrays along the 3rd axis i.e. axis = 2
*   Use numpy.where() function to split arrays at a specific value

* * *