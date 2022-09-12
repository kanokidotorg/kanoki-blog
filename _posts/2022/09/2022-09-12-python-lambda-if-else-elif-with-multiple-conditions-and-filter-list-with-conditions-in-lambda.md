---
title: "python lambda if, else & elif with multiple conditions and filter list with conditions in lambda"
date: "2022-09-12"
categories: [ python]
tags: [python]

---

lambda is an one-liner python functions used to quickly built a function with ease, In this post we would see how to use if-else or multiple conditions to evaluate an expression using lambda functions.

Also, we will see how to filter a list by applying conditions in a loop and filter in lambda function

## Lambda if-else & elif with multiple conditions
We can use if-else and elif and multiple conditions using lambda function in Python.

Let's take an example to understand how to use conditions in lambda

**Condition:**

we want to assign grade based on the scores obtained in a test, here is a condition for assigning the grades:

```python
if grade > 7:
    A+
elif grade >= 5 and grade <= 7:
    A
else grade <5:
    B
```

We can write this multiple conditions in lambda as shown below

```python
grade = lambda x: 'A+' if x>7 else 'A' if x>=5 and x<=7 else 'B'
```

```python
grade(8)
Out: 'A+'
```

Alternatively, we could also write the above conditions using lambda like as shown below

```python
lambda x : (false,true)[Condition]
```

```python
grade = lambda score : ((((('Not sure' , 'B')[score < 5]), 'A')[(score>=5 | score<=7)]), 'A+')[score>7]
```

```python
grade(8)
Out: 'A+'
```



## Conditional Statement in Lambda

We could also evaluate a statement if it's True or False using Lambda.

Let's verify if this condition is True or False

```python
if grade > 7 and grade <=10 or grade <=5:
   print(True)
```

**Using Lambda:**

We can use bitwise operator in lambda to create the condition, Here we want to verify if score is greater than 7 and less than 10 or if it is less than 5 or not

```python
grade = lambda x: ((x>7) & (x<=10))| (x<=5)
grade(12)
Out: False
```

## Lambda else do nothing

We could also write only the if statement without an else like this

A lambda, like any function, must have a return value. Without else does not work because it does not specify what to return if not x>7

```python
f = lambda x: x**3 if (x > 7) else None
```

## lambda filter list with multiple conditions

Filtering lists with lambda will return all the elements of the list that return True for the lambda function

```python
data = [10, 9, 7, 0]
```

The conditions are joined by `and`, they return `True` only if all conditions are `True`, and if they are joined by `or`, they return `True` when the first among them is evaluated to be `True`

```python
list(filter(lambda x: (x%2==0)&(x>=5), data))
Out: [10]
```

In this case the only item in list that satisfied the condition is returned

## lambda evaluate list with multiple conditions inside a loop

We could also use lambda to loop over a list and evaluate a condition

Let's define a function with all the conditions that we want to evaluate

```python
def f(grade):
    if grade > 7:
        return 'A+'
    elif grade >= 5 and grade <= 7:
        return 'A'
    elif grade <5:
        return 'B'
    else:
        return False
```

```python
data = [10, 9, 7, 0]
```

Will define a lambda function to map each item in the data list to this function

```python
grade = lambda x: list(map(f, x))
```

Let's evaluate the grade for each of the items in the data list

```python
grade(data)
Out: ['A+', 'A+', 'A', 'B']
```

