---
title: "How to iterate through a python dictionary"
date: "2019-12-04"
---

A python Dictionary is one of the important data structure which is extensively used in data science and elsewhere when you want to store the data as a key-value pair. In this post we will take a deep dive into dictionaries and ways to iterate over dictionary and find out how to sort a dictionary values and other operations using dictionary data structure

Basically a dictionary in python is a mapping object where it maps a key to a value. The keys are hashable values which are mapped to values. The keys are arbitrary values and any values that are not hashable that is values containing list or other mutable types may not be used as Keys. Even it is not a good practice to use numeric values as keys also.

## **How to create a Dictionary?**

We can create a Dictionary using key:value pairs separated by commas or using the dict constructor

### **Using comma separated key value pair**

d = {  
"name": "Charlie",  
"age": 42,  
"city": "NYC",  
"state": "NY",  
"rank": 1.0 }

### **Using dict constructor**

```
d = dict( name= "Charlie", age= 42, city = "NYC", state = "NY",rank = 1.0)
```

### **Convert two list into a dictionary**

```
d = dict( zip(['name','age','city','state','rank'],['Charlie',42,'NYC','NY',1.0]))
```

### **Convert list of tuples(Key,Value) into a dictionary**

```
d = dict([("name","charlie"),("age",42),("city","NYC","state","NY"),("rank",1.0)])
```

## **What's Changed for Dictionary in Python 3.6**

Dictionaries got ordered in Python 3.6 that means it remembers the order of insertion of key and value pairs. It means that keyword arguments can now be iterated by their creation order, which is basically the cpython implementation of python

The memory usage of this new dictionary implementation will also reduce the memory usage by 20-25%

Here is an example from the python dev [mailing list](https://mail.python.org/pipermail/python-dev/2016-September/146327.html) for the implementation of new dict in python 3.6

Create a function to get the dictionary keys

```
def func(**kw):
  print(kw.keys())
```

Calling the above function in Python 3.5 and before returns an un-ordered dict\_keys. Check the output the keys are randomly ordered

```
func(a=1, b=2, c=3, d=4, e=5)
```

**Output in Python 3.5 and before:**

dict\_keys(\['c', 'd', 'e', 'b', 'a'\]) # Keys are randomly ordered

Calling the same function in python3.6 and above returns a dict\_keys in the same order as it has been passed in the function

**Output in Python 3.6 and above:**

dict\_keys(\['a', 'b', 'c', 'd', 'e'\]) # Keys are ordered

## **Iterating thru a dictionary**

As a python developer or data scientists you will be working on dictionaries a lot and there are calculations or actions that needs to be performed while working through the dictionary keys and values

In this section we will see what are the ways you can retrieve the data from the dictionary

Python supports a concept of iteration over containers and An iterator needs to define two methods: **iter**() and **next**(). Usually, the object itself defines the **next**() method, so it just returns itself as the iterator.

the **iter** defines the next method which will be called by statements like for and in to yield the next item, and next() should raise the StopIteration exception when there are no more items

Hope this clears how the iterator works on the dictionary

These methods are used by for and in statements, so what it means is if you put a dictionary under a for loop then it will automatically call the **iter**() method and iterate over the dictionaries keys

### **Iterate dictionary using keys**

```
for keys in d:
    print(keys, ":", d[keys])
```

**Output:**

name : charlie  
age : 42  
city : NYC  
state : NY  
rank : 1.0

## **Dictionary view objects**

This provide a window of the dictionary entries and when any of the item changes in the dict then it will reflect those changes in the views

As per the python official documentation:

> The objects returned by dict.keys(), dict.values() and dict.items() are view objects. They provide a dynamic view on the dictionary’s entries, which means that when the dictionary changes, the view reflects these changes

### **Length of Dictionary**

This returns number of key,value pairs in the dictionary

```
len(d)
```

### **list of dictionary keys**

the keys() function returns dict\_keys() which is wrapped with the list to get a list object.

```
 list(d.keys()) 
```

**Output:**

\['name', 'age', 'city', 'state', 'rank'\]

### **list of dictionary values**

the values() function returns dict\_values() which is wrapped with the list to get a list object

```
list(d.values())
```

**Output:**

\['charlie', 42, 'NYC', 'NY', 1.0\]

### **Iterate thru dict values**

The dict\_values is an iterable so you can directly iterate through the values of dictionaries using for loop

```
for value in d.values():
      print(value)
```

**Output:**

Charlie  
42  
NYC  
NY  
1.0

### **Iterate thru dict keys**

The dict\_keys is an iterable so you can directly iterate through the keys of dictionaries using for loop

```
for key in d.keys():
     print(key) 
```

**Output:**

name  
age  
city  
state  
rank

### **Iterate key, value pair using dict.items**

This returns a (key,value) pair which is a tuple object.

```
for key,value in d.items():
         print(key,":",value)
```

**Output:**

name : Charlie  
age : 42  
city : NYC  
state : NY  
rank : 1.0

## **Dictionary Comprehension**

It is a syntactical extension to list comprehension. It produce a dictionary object and you can't add keys to an existing dictionary using this

You group the expression using curly braces and the left part before the for keyword expresses both a key and a value, separated by a colon

```
city = ['NYC', 'Boston', 'Los Angeles']
state = ['NY', 'Massachussets', 'California']
city_state_dict = {key: value for key, value in zip(state, city)}
```

**Output:**

{'California': 'Los Angeles', 'Massachussets': 'Boston', 'NY': 'NYC'}

## **Dictionary Get Key**

> get(key,default)

This method returns the value for _key_ if _key_ is in the dictionary, else _default_. If _default_ is not given, it defaults to `None`, so that this method never raises a [`KeyError`](https://docs.python.org/3/library/exceptions.html#KeyError).

Let's understand this with a small example

Here is a dictionary of city and respective states:

> city\_state\_dict = {  
> "NYC": "New York",  
> "Boston": "Massachussets",  
> "Charlotte": "North Carolina",  
> "Los Angeles": "California",  
> "Minnetonka": "Minnesota" }

We have to output the state name when user enters a city

When city is in Dictionary Key:

```
city = "Charlotte"
print(f'You live in state of {city_state_dict.get(city)}')
```

**Output:**

You live in state of **North Carolina**

When city is not in Dictionary Key:

```
city = "Raleigh"
print(f'you live in state of {city_state_dict.get(city)}')
```

**Output:**

You live in state of **None**

## **Unpack a Dictionary using itemgetter**

You can unpack a dictionary using operator.itemgetter() function which is helpful if you want to get the values from the dictionary given a series of keys

Look at this example below we are extracting the values of keys a, b and c using itemgetter

```
from operator import itemgetter
d = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
a, b, c = itemgetter('a', 'b', 'c')(stuff)
print(a,b,c)
```

## **Sorting**

There are certain situations where you need to sort the dictionary either by key or values. You can achieve this using sorted() function. In the below section we will see how to sort the dictionary by keys and values

if you are using python 3.6 and above then do not worry the dictionary are ordered data structured and can be easily sorted using sorted() function inside a dictionary comprehension

### **Sorting Dictionary by Keys**

You can pass the entire dictionary dict\_items as an argument to sorted function and it will return a list of tuples after sorting which can be converted back to dictionary using the dict constructor

```
d = {'B':10,'A': 4}
dict(sorted(d.items()))
```

**Output:**

{'A': 4, 'B': 10}

You can see the output the dictionary keys are sorted alphabetically

### **Sorting Dictionary by Values**

You can also sort the dictionary with their values using the sorted() function and another argument key and value of the _key_ parameter should be a function that takes a single argument and returns a key to use for sorting purposes

By default it gives the result in ascending order. You can see the list of keys that is returned here based on the values of each of the corresponding keys arranged in ascending order

**Ascending Order**

```
d = {'a':1000, 'b':3000, 'c': 100}
sorted(d, key=d.get)
```

**Output:**

\['c', 'a', 'b'\]

if you want the list of keys to be in descending order above then add another argument called reverse as True and this will give the keys based on their values arranged in descending order

**Descending order**

```
d = {'a':1000, 'b':3000, 'c': 100}
sorted(d, key=d.get, reverse=True)
```

**Output:**

\['b', 'a', 'c'\]

## **Enumerate Dictionary**

You can also enumerate through the dictionary and can get the index of each of the key

In the final output you can see the index for each of the key .

Just remember d.keys(), d.values() returns a dict\_keys object which behaves a lot more like a set than a list

Therefore, dict.values() needs to be wrapped in a list. You can see in the below code we have used **list(d.values())\[index\]**

```
d = {'a':1000, 'b':3000, 'c': 100, 'd':542,'e':790, 'f': 1042}

for index,item in enumerate(d):
    print(index," ",item," : ",list(d.values())[index])
```

**Output:**

```
0   a  :  1000
1   b  :  3000
2   c  :  100
3   d  :  542
4   e  :  790
5   f  :  1042
```

## **Filter Dictionary**

You can filter a dictionary using the dictionary comprehension which will create a new dictionary based on the filter criteria.

In this example we are filtering out all the key-value pairs where the value is < 500. The final dictionary have all the keys with values greater than 500

```
d = {'a':1000, 'b':3000, 'c': 100, 'd':542,'e':790, 'f': 1042}
{k:v for k,v in d.items() if v > 500}
```

**Output:**

{'a': 1000, 'b': 3000, 'd': 542, 'e': 790, 'f': 1042}

## **Update Dictionary value**

The update method overwrites the existing keys and Update the dictionary with the key/value pairs

It accepts either a dictionary object or an iterable of key/value pairs

```
d = {'a':1000, 'b':3000, 'c': 100, 'd':542,'e':790, 'f': 1042}
d.update(g=790,b=3049)
```

**Output:**

{'a': 1000, 'b': 3049, 'c': 100, 'd': 542, 'e': 790, 'f': 1042, 'g': 790}

## **Delete Dictionary Key**

you can use del to delete a key and it's value from a dictionary if you are sure key exists in the dictionary else it Raises a [`KeyError`](https://docs.python.org/3/library/exceptions.html#KeyError) if _key_ is not in the map

```
d = {'a':1000, 'b':3000, 'c': 100, 'd':542,'e':790, 'f': 1042}
del d['f']
```

**Output:**

{'a': 1000, 'b': 3000, 'c': 100, 'd': 542, 'e': 790}

### **dict.pop()**

If you aren't sure about the key exists in dictionary then use dict.pop().

This will return d\['f'\] if key exists in the dictionary, and None otherwise.

If the second parameter is not specified (i.e. d.pop('f')) and key does not exist, a KeyError is raised.

```
d.pop('f', None)
```

## **Merge two or more Dictionaries**

You can use the dictionary unpacking operator \*\* to merge two or more dictionary into one single dictionary and then you can iterate this new merged dictionary

Here is an example:

```
city_1 = {'NYC':'NY','Boston':'Massachussets'}
city_2 = {'Los Angeles': 'California', 'Charlotte': 'North Carolina'}

# Merge Two Dictionaries
merge_city_1_2 = {**city_1,**city_2}
```

**Output:**

{'NYC': 'NY', 'Boston': 'Massachussets', 'Los Angeles': 'California', 'Charlotte': 'North Carolina'}

## **Conclusion:**

We have reached to the end of this comprehensive and detailed post on iterating through dictionaries. So here is what we have learned so far through our journey thru this blog

1. What are Dictionaries and how it works and what are different ways to create a dictionary in python
2. Dictionary view objects, why it is important and it's dynamic nature to reflect the changes immediately
3. Dictionary get function to retrieve the values from a simple and nested dictionary and it returns None if the key is not found
4. How to sort a dictionary by it's keys and values in ascending and descending orders
5. Enumerate thru a dictionary and get the index position of it's keys which are ordered now after python 3.6
6. Finally, what are the different methods to update and delete a dictionary key

I have tried to cover all the possible ways to iterate through a dictionary and explain different ways to work with dictionary efficiently.

Still if there are anything you feel should be included in this post or can be done in more optimized way then please leave a comment below.

* * *
