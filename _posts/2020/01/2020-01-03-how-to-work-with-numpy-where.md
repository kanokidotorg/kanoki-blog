---
title: "How to work with numpy.where()"
date: "2020-01-03"
categories: [ Data Science, numpy, Python, Python, Data Science ]
tags: [ DataScience, numpy, Python ]
---

## **What is numpy.where()**

> **numpy.where(condition\[, x, y\])**
> Return elements chosen from x or y depending on condition

if condition is true then x else y

### **parameters**

**x, y : array\_like**

### **Output**

Output is a ndarray

An array with elements from _x_ where _condition_ is True, and elements from _y_ elsewhere

## **how to use numpy.where()**

First create an Array

```
x = np.arange(9)
```

**Output**

array(\[ 0,  1,  4,  9, 16, 25,  6,  7,  8\])

Now we will Square all the elements in array which is greater than 5 ( x>5 )

```
np.where(x>5, x**2, x)
```

**Output**

array(\[ 0,  1,  2,  3,  4,  5, 36, 49, 64\])

The output is a ndarray where all the elements >5 are squared and elements <5 remains same

## **numpy.where() with 2D array**

First create a 3X3 matrix

```
import numpy as np
x = np.arange(9.).reshape(3, 3)
```

**Output**

array(\[\[0., 1., 2.\],
       \[3., 4., 5.\],
       \[6., 7., 8.\]\])

Now Find all the elements in this matrix which is greater than 5

```
np.where(x>5)
```

Note: we are not giving any value for x and y here, just passing the condition i.e. x>5

**Output**

(array(\[2, 2, 2\], dtype=int64), array(\[0, 1, 2\], dtype=int64))

The output is 2 1D arrays containing index of matching rows and columns

if you check the following elements in original array they will be greater than 5

(2,0) is 6 , (2,1) is 7 and (2,2) is 8

if you want the elements directly then

```
x[np.where( x > 5 )]
```

**Output**

array(\[6., 7., 8.\])

## **numpy.where() with boolean array as condition**

Create list of some values

values = \[3, 4, 7\]

Get boolean array where original array contains the elements in values list above

```
ix = np.isin(x, values)
ix
```

**Output:**

array(\[\[False, False, False\],
       \[ True,  True, False\],
       \[False,  True, False\]\])

Now we will feed this output to numpy.where to get all the values in original array matching the elements in value list

```
x[np.where(ix)]
```

**Output**

 array(\[3., 4., 7.\])

Output is the list of elements in original array matching the items in value list

## **Conclusion**

In this post we have seen how numpy.where() function can be used to filter the array or get the index or elements in the array where conditions are met

Additionally, We can also use numpy.where() to create columns conditionally in a pandas datafframe
