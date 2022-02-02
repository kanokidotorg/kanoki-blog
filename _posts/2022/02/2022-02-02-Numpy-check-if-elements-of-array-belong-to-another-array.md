---

title: "Numpy check if elements of array belong to another array"
date: "2022-02-02"
categories: [ python, numpy,]
tags: [ python, numpy]

---

In this article we are going to see what are the convenience functions that can be used to check if elements of a 1D array exists in another 2D array or not

we will be just following these steps in order to check those elements belongs to another array:

1. Create a 1D array(X) and 2D array(Y)
2. Use *numpy.isin()* to return a boolean array of the same shape as Y that is True where an element of Y is in X and False otherwise
3. Use *numpy.in1d()* to test whether each element of a 1-D array X is also present in a second array Y
4. Index of Y where the elements match with the 1D array X

Let's get started

## Create a 1D array X and 2D array Y
Let's create a 1D array X using numpy random to generate random integers of size 4 and create a second 2D array with random integers of shape 4x4

```python
x = np.random.randint(0, 9, size=(4))
x
```

Out: array([6, 4, 2, 7])


```python
y=np.random.randint(0, 9, size=(4, 4))
y
```

```python
Out: 
array([[5, 8, 7, 8],
       [3, 2, 2, 3],
       [6, 0, 3, 7],
       [8, 6, 0, 3]])              
```
We will check if elements in X belongs to array Y or not

## Use numpy.isin() to find elements in 1D array X exists in 2D array Y

*numpy.isin()* is an element-wise function version of the python keyword *in*. It calculates element in array X, broadcasting over 2D array Y only. Returns a boolean array of the same shape as 2D array Y that is True where an element of element is in 1D array X and False otherwise

In the output boolean array you can see all the positions where elements from 1D array exists is set as True and False otherwise

```python
np.isin(Y, X)
```

```python
Out: 
array([[False, False,  True, False],
       [False,  True,  True, False],
       [ True, False, False,  True],
       [False,  True, False, False]])            
```

You can also invert the test by setting invert parameter to True

``python
np.isin(Y, X, invert=True)
```

​```python
Out: 
array([[ True,  True, False,  True],
       [ True, False, False,  True],
       [False,  True,  True, False],
       [ True, False,  True,  True]])            
```

We can also check which all elements in 2D array Y belongs to 1D array X by just changing the position of X and Y in the *isin()* function. In this case a 1D array of shape X is returned

```python
np.isin(X, Y)
```

```python
Out: 
array([ True, False,  True,  True])           
```

## Use numpy.in1d() to find elements in 1D array X exists in 2D array Y

*numpy.in1d()* tests whether each element of a 1-D array is also present in a second array.

Returns a boolean array the same length as X that is True where an element of X is in Y and False otherwise.

Numpy official documentation recommends using *isin()* over *in1d()*

```python
np.in1d(X, Y)
```

```python
Out: 
array([ True, False,  True,  True])           
```

## Find index of the elements of 1D array X in 2D array Y

we have seen so far all the functions that gives a booelan array to test if element of one array exists in another array or not. 

What next? how can you find the indices of all those elements in 1D array in the 2D array

You can use *numpy.where()* along with *numpy.isin()* as shown below

```python
np.where(np.isin(y,x))
```

```python
Out: 
(array([0, 1, 1, 2, 2, 3]), array([2, 1, 2, 0, 3, 1]))          
```

Or, pass the boolean output of *numpy.isin()* in *numpy.nonzero()* to return the index of elements in 2d array Y that matches the element in 1D array X


```python
boolean_mask = np.isin(Y, X)
boolean_mask
```

```python
Out: 
array([[False, False,  True, False],
       [False,  True,  True, False],
       [ True, False, False,  True],
       [False,  True, False, False]])            
```

Get index where the elements are true in above mask

```python
np.nonzero(boolean_mask)
```
```python
(array([0, 1, 1, 2, 2, 3]), array([2, 1, 2, 0, 3, 1]))
```

## Conclusion:

Here is the brief summary of what we have learned in this post:

- Use *numpy.isin()* to find the elements of a array belongs to another array or not. it returns a boolean array matching the shape of other array where elements are to be searched
- *numpy.isin()* also inverts the result by using invert parameter and setting it up as True
- For a 1D test array we can also use in1d() to check whether the elements of 1D array belongs to another array
- Use *numpy.where()* and *numpy.isin()* to get the index of all the matching elements in other array
- Alternatively, we can also use the boolean output of *numpy.isin()* in *numpy.nonzero()* to find the index of all matching elements


