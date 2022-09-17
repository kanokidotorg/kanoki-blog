---
title: "Python Dictionary Append"
date: "2022-09-17"
categories: [ python]
tags: [python]
last_modified_at: 2022-09-18
permalink: python-dictionary-append-key-value-pair

---

In this post we will discuss how to append a new key-value pair or update an existing key value in a dictionary. There are two ways you can append or update the values in a dictionary:

- d[key] = value
- d.update(key:value)

In python 3.5+ you can also use dictionary unpacking to merge two dictionaries and in python 3.9+ we can use merge(\|) and update(\|\=) operator for dictionaries

## Adding a new key-value pair
First create a dictionary to work with

```python
my_dict = {
           'USA': 'Washington dc',
           'China': 'Beijing',
           'Germany': 'Berlin',
           'France': 'Paris'
          }
```

Now let's see different ways to append a new key-value pair to this dictionary

Using the subscript notation like `d[key]=val` syntax as it is shorter and can handle any object as key (as long it is hashable), and only sets one value

```python
my_dict['Russia']='Moscow'
my_dict
```

```python
Out: 
  {
           'USA': 'Washington dc',
           'China': 'Beijing',
           'Germany': 'Berlin',
           'France': 'Paris',
           'Russia': 'Moscow'
  }
```

Alternatively, we could also use magic method \_\_setitem\_\_ but it's very poor in performance so avoid using this to add a single key-value pair

```python
my_dict.__setitem__('Russia', 'Moscow')
my_dict
```

```python
Out: 
  {
           'USA': 'Washington dc',
           'China': 'Beijing',
           'Germany': 'Berlin',
           'France': 'Paris',
           'Russia': 'Moscow'
  }
```



## Add or Update Multiple key-value pairs

We want to update multiple key-value pairs then dictionary.update() method is suitable for it.

```python
my_dict.update({'Russia':'Moscow', 'Belarus': 'Minsk'})
my_dict
```

```python
Out:
  {
         'USA': 'New York',
         'China': 'Beijing',
         'Germany': 'Berlin',
         'France': 'Paris',
         'Russia': 'Moscow',
         'Belarus': 'Minsk'
  }
```



**Using dictionary unpacking for merging two dictionary:**

We could also use ** kwargs to merge two dictionary in python 3.5+, It's called dictionary unpacking and syntax is as follows: data = { \*\*data1, \*\***data2, \*\*data3}.  The \*\*  turns the dictionary into keyword parameters.

The dictionary (or dictionary-like) object passed with `**kwargs` is expanded into keyword arguments to the callable, much like `*args` is expanded into separate positional arguments

```python
{**my_dict, **{'Russia':'Moscow', 'Belarus': 'Minsk'}}
```

## **Using update and merge operator for  dictionary in  python 3.9+**

As of Python3.9, the merge operator \| works for dictionaries as well

```python
my_dict | {'Russia':'Moscow', 'Belarus': 'Minsk'}
```

```python
Out: 
  {
        'USA': 'Washington dc',
        'China': 'Beijing',
        'Germany': 'Berlin',
        'France': 'Paris',
        'Russia': 'Moscow',
        'Belarus': 'Minsk'
  }
```



Also, python3.9+ let use the update operator \|\= for dictionaries

```python
my_dict |= {'USA':'New York', 'Greece':'Atehns'}
my_dict
```

```python
Out: 
  {
        'USA': 'New York',
        'China': 'Beijing',
        'Germany': 'Berlin',
        'France': 'Paris',
        'Greece': 'Athens'
  }
```



## Dictionary append value if key exists

First we will check if key already exists in a dictionary or not. we could either used dict.get() or check if key is available in dict.keys() list

```python
my_dict.get('Poland', 'NA')
Out:
  'NA'
```

This will throw 'NA' because the key Poland is not available in original dictionary my_dict

We could also check the key in dict.keys() list:

```python
'China' in my_dict.keys()
```

Next we could update the value using the subscript notation or update() method if the key exists or otherwise create a new key-value pair

```python
if 'Poland' not in my_dict.keys():
    my_dict['Poland'] = 'Warsaw'
    
# OR

if not my_dict.get('Poland'):
    my_dict.update('Poland','Warsaw')
 
my_dict
```

```python
Out:
  {
         'USA': 'Washington dc',
         'China': 'Beijing',
         'Germany': 'Berlin',
         'France': 'Paris',
         'Poland': 'Warsaw'
  }
```



## Append to dictionary in for loop

Let's take two list and append the country-cities as key-value pair in the original dictionary my_dict in a for loop

```python
list_country = ['Brazil', 'Italy', 'Nigeria', 'India']
list_cities = ['Brasilia', 'Rome', 'Abuja', 'New Delhi']
```

We will zip the two list and iterate through it and append the value in the dictionary

```python
for country,city in zip(list_country, list_cities):
    my_dict[country] = city
```

```python
my_dict

Out:
  {
         'USA': 'Washington dc',
         'China': 'Sanghai',
         'Germany': 'Berlin',
         'France': 'Paris',
         'Russia': 'Moscow',
         'Brazil': 'Brasilia',
         'Italy': 'Rome',
         'Nigeria': 'Abuja',
         'India': 'New Delhi'
  }
```



