---
title: "Numpy to generate random number between 0 and 1"
date: "2022-09-28"
categories: [python, numpy]
tags: [python, numpt]
last_modified_at: 2022-09-28
permalink: numpy-to-generate-random-number-between-0-and-1

---

In this post we will discuss how to quickly generate random numbers and float between 0 and 1 or between a range using numpy.

Why we are using numpy to generate random numbers? Because numpy arrays can be easily integrated with Pandas and we can generate dataframe columns with these random numbers too.

Numpy has these three functions that can be used to generate the random number and floats between a range

- numpy.random.uniform
- numpy.random.randint
- numpy.random.sample

## <span style="color:blue">1. Generate Random number between 0 and 1</span>

### <span style="color:brown;font-size: 95%">a) numpy.random.uniform():</span>

```python
np.random.uniform(low, high, size)
```

**low:** Lower boundary of the output array, All values generated will be greater than or equal to low. The default value is 0

**high:** Upper boundary of the output array, All values generated will be less than or equal to high. The default value is 1.0

**size:** Output shape or length of array

<span style="color:blue;font-size: 90%">1. Generate 20 random float between 0 and 1:</span>

```python
import numpy as np
np.random.uniform(0, 1, size=20)
```

```python
Out:
  array([0.69203392, 0.34019562, 0.74442452, 0.9980623 , 0.35615059,
         0.33135049, 0.13165237, 0.43444599, 0.52700622, 0.64007571,
         0.13452034, 0.5095588 , 0.82088138, 0.26085551, 0.00696426,
         0.40988165, 0.43614663, 0.35560554, 0.97111713, 0.71243258])
```

### <span style="color:brown;font-size: 95%">b) numpy.random.sample():</span>

It returns the random floats in the half-open interval [0.0, 1.0)

```python
np.random.sample(size)
```

**size:** Output shape or length of array

<span style="color:blue;font-size: 90%">1. Generate 20 random float between 0 and 1:</span>

```python
np.random.sample(20)
```

By default it outputs values between 0 and 1

```python
Out:
array([0.83622401, 0.27656984, 0.51956385, 0.58562562, 0.81212898,
       0.52548523, 0.7759998 , 0.44796633, 0.73089862, 0.99077859,
       0.79913039, 0.7595591 , 0.948321  , 0.99220847, 0.37780327,
       0.87530571, 0.13907738, 0.00785441, 0.53615096, 0.61039024])
```



## <span style="color:blue">2. Generate random number between range</span>

### <span style="color:brown;font-size: 95%">a) Random Integer between range</span>

numpy.random.randint returns random integers from *low* (inclusive) to *high* (exclusive)

```python
np.random.randint(low, high, size, dtype)
```

**low:** Lowest (signed) integers to be drawn from the distribution, if high= None then this parameter is one above the *highest* such integer

**high:** If provided, one above the largest (signed) integer to be drawn from the distribution

**size:** Output shape or length of array. If the given shape is, e.g., `(m, n, k)`, then `m * n * k` samples are drawn. Default is None, in which case a single value is returned

<span style="color:blue;font-size: 90%">1.  Generate 20 random integer between 0 and 10 </span>

```python
np.random.randint(10, high=None, 20)
```

Out:

```python
array([7, 4, 6, 0, 7, 0, 9, 9, 3, 2, 9, 6, 8, 8, 0, 1, 1, 6, 1, 8])
```

<span style="color:blue;font-size: 90%">2. Generate random 10 integer between 5 and 20</span>

```python
np.random.randint(low=5, high=20, size= 10)
#or
np.random.randint(5, 20, 10)
```

Out:

```python
array([ 5,  7, 13,  7, 19,  6, 11, 12, 17,  7])
```

<span style="color:blue;font-size: 90%">3. Generate a 2D array between 0 and 8 of size 2x4</span>

```python
np.random.randint(8, size=(2, 4))
```

Out:

```python
array([[6, 7, 6, 6],
       [0, 5, 3, 0]])
```

<span style="color:blue;font-size: 90%">4. Generate 1D array between 1 and 3 different upper bounds</span>

```python
np.random.randint(1, [3, 5, 10])
```

Out:

```python
array([1, 4, 6])
```

<span style="color:blue;font-size: 90%">5. Generate 1D array between 3 lower bounds and 3  upper bounds</span>

```python
np.random.randint([1, 3, 4], [3, 5, 10])
```

Out:

```python
array([2, 3, 4])
```

<span style="color:blue;font-size: 90%">6. Generate 2d array of size 2x3, where </span>

```python
np.random.randint([8,0, 8],[[10], [9]])
```

Out:

```python

array([[9, 6, 9],
       [8, 1, 8]])
```

<span style="color:blue;font-size: 90%">7. Generate another 2D array of size 3x2 </span>

```python
np.random.randint([0,8],[[9], [20], [10]])
```

Out:

```python
array([[ 3,  8],
       [ 6, 10],
       [ 7,  9]])
```



### <span style="color:brown;font-size: 95%">b) Random float/decimals between range</span>

<span style="color:blue;font-size: 90%">1. Generate 20 float values between 0 and 10 using random.uniform </span>

```python
np.random.uniform(0, 10, 20)
```

Out:

```python
array([2.52003182, 7.92110332, 1.50855153, 9.90953893, 2.03089333,
       1.70043576, 5.4130004 , 2.76550054, 3.66412313, 0.43711276,
       6.86533095, 7.87814796, 5.87564616, 1.2567026 , 7.95171546,
       3.30952517, 9.29987975, 1.10395478, 9.94582268, 5.83766228])
```

<span style="color:blue;font-size: 90%">2. Generate 2D 2x2 float array  </span>

```python
np.random.uniform([0,8],[[9], [20]])
```

Out:

```python
array([[ 2.56164838,  8.77214394],
       [ 3.93668152, 13.50095773]])
```

<span style="color:blue;font-size: 90%">3. Generate 20 float values between 5 and 10 using random.sample</span>

Random.sample returns random floats between 0 and 1, so in order to get the values between a range, in this case it is between 5 and 10

We would multiply the random values by upper bounds and add lower bound to it

```python
(high-low) * np.random.sample(size=20) + low
```

Let's put the low and high value as 5 and 10 respectively.

```python
(10-5) * np.random.sample(size=20) + 5
```

Out:

```python
array([5.30887252, 5.68872096, 8.84929092, 6.92724732, 5.45294085,
       7.11049378, 8.53911988, 9.43878303, 7.81624595, 7.77538502,
       6.88726948, 8.08815515, 5.81105502, 9.65732916, 7.23508785,
       8.0566595 , 9.36133479, 8.2490744 , 5.60270196, 6.72205368])
```

## <span style="color:blue">3. Pandas create dataframe of random numbers</span>

```python
import pandas as pd
df = pd.DataFrame(np.random.uniform(0,100,size=(5, 4)), columns=list('ABCD'))
```

Out:

|      |     A     |     B     |     C     |     D     |
| :--: | :-------: | :-------: | :-------: | :-------: |
|  0   | 10.761108 | 0.856031  | 18.205962 | 7.594729  |
|  1   | 14.689869 | 12.605781 | 9.522636  | 0.307509  |
|  2   | 11.682856 | 11.915566 | 2.554456  | 1.212645  |
|  3   | 1.757673  | 10.225725 | 4.179541  | 8.477603  |
|  4   | 0.429727  | 2.289190  | 14.142173 | 14.396024 |

