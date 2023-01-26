---
title: "How to Append dictionary to a dataframe in pandas"
date: 2023-01-26
categories: [pandas, python]
tags: [pandas, python]
last_modified_at: 2023-01-26
permalink: how-to-append-dictionary-to-a-dataframe-in-pandas

---

In this article, we will see how to append a dictionary to a dataframe, we could either append dictionary as rows or as a new column to the dataframe 

Here are the steps that we will follow in this article to append dictionary as either row or column

- Create a test dataframe
- Create a dictionary of new row values
- Build a dataframe with the dictionary row values using pandas ***from_dict()*** function
- Append dictionary dataframe to the test dataframe using pandas ***concat()***

* Create another dictionary with the list of values in a column
* Append dictionary as column to the test dataframe

## Create a dataframe

We will create a test dataframe with student details. It has got three columns - Name, Grade and Score.

```python
# importing required libraries
import pandas as pd
import numpy as np

# creating dataframe
df = pd.DataFrame(
        {'student': ['Alex', 'Sam', 'Mary', 
                     'Kayle', 'Paige'],
         'grade': ['A', 'A', 'B', 'C', 'A'],
         'score': [45, 48, 39, 35, 49]})

df
```

This is how our school dataframe looks like:

|      | Student | Grade | Score |
| :--: | :-----: | :---: | :---: |
|  0   |  Alex   |   A   |  45   |
|  1   |   Sam   |   A   |  48   |
|  2   |  Mary   |   B   |  39   |
|  3   |  Kayle  |   C   |  35   |
|  4   |  Paige  |   A   |  49   |

## Append Dictionary as row to dataframe

#### New Dictionary:

We will create a dataframe with all the new row values that we want to append to the test dataframe created above.

These are the three new students and their corresponding row values, the key of dictionary is the index and the values in list is the row values for each column student, grade and score respectively

```python
new_students = { 5:['stehany', 'C', 32], 
                 6:['Jon', 'A', 47], 
                 7:['Charles', 'B', 40]}
```

Next, we want to create a dataframe for these new students and let's call it df_new_students, we will use [pandas.from_dict()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.from_dict.html) to build this dataframe from the above dictionary

[pandas.from_dict()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.from_dict.html) function creates DataFrame object from dictionary by columns or by index, we have selected the *orient* parameter as *index* since we want to add these dictionary values as rows

#### Dictionary to dataframe:

```python
df_new_students = pd.DataFrame.from_dict(new_students, 
                       orient='index', 
                       columns=['student', 
                                'grade', 
                                'score'])
df_new_students
```

|      | Student | Grade | Score |
| :--: | ------: | ----: | ----: |
|  5   | stehany |     C |    32 |
|  6   |     Jon |     A |    47 |
|  7   | Charles |     B |    40 |

#### Append Dictionary dataframe to test dataframe:

We will now append the dictionary to test dataframe using [pd.concat()](https://pandas.pydata.org/docs/reference/api/pandas.concat.html) function, It concatenates pandas object along a particular axis

```python
newdf = pd.concat([ df, df_new_students])
newdf
```

|      | Student | Grade | Score |
| :--: | :-----: | :---: | :---: |
|  0   |  Alex   |   A   |  45   |
|  1   |   Sam   |   A   |  48   |
|  2   |  Mary   |   B   |  39   |
|  3   |  Kayle  |   C   |  35   |
|  4   |  Paige  |   A   |  49   |
|  5   | stehany |   C   |  32   |
|  6   |   Jon   |   A   |  47   |
|  7   | Charles |   B   |  40   |

## Append Dictionary as column to dataframe

Let's create a new dictionary that we want to add as a new column to the test dataframe

```python
ethnicity_col = { 
              'ethicity':
                   [
                    'Asian', 'American', 
                    'Hispanic', 'African-American', 
                    'Asian-American', 'American', 
                    'Hispanic', 'African-American'
                   ]
                }
```

Add this dictionary as [pd.series](https://pandas.pydata.org/docs/reference/api/pandas.Series.html) and create a new column called ethicity

```python
newdf['ethnicity'] = pd.Series(ethnicity_col['ethicity'])
```

|      | Student | Grade | Score |    Ethnicity     |
| :--: | :-----: | :---: | :---: | :--------------: |
|  0   |  Alex   |   A   |  45   |      Asian       |
|  1   |   Sam   |   A   |  48   |     American     |
|  2   |  Mary   |   B   |  39   |     Hispanic     |
|  3   |  Kayle  |   C   |  35   | African-American |
|  4   |  Paige  |   A   |  49   |  Asian-American  |
|  5   | stehany |   C   |  32   |     American     |
|  6   |   Jon   |   A   |  47   |     Hispanic     |
|  7   | Charles |   B   |  40   | African-American |

Let's create another dictionary with column values and we will add this as a new column to the dataframe

```python
zodiac_sign = { 
                'zodiac':
                   [
                    'capricorn', 'Sagittarius', 
                    'Pisces', 'Scorpio', 
                    'Libra', 'Virgo', 
                    'Leo', 'Cancer'
                   ]
              }
```

We will now add this dictionary as a new column to this dataframe(newdf) using [pandas.from_dict()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.from_dict.html), the *orient* paramter will be changed to *columns* this time

```python
newdf['zodiac_sign'] = newdf.from_dict(
                          zodiac_sign, 
                          orient='columns')
newdf
```

|      | Student | Grade | Score |    Ethnicity     | Zodiac_sign |
| :--: | :-----: | :---: | :---: | :--------------: | :---------: |
|  0   |  Alex   |   A   |  45   |      Asian       |  Capricorn  |
|  1   |   Sam   |   A   |  48   |     American     | Sagittarius |
|  2   |  Mary   |   B   |  39   |     Hispanic     |   Pisces    |
|  3   |  Kayle  |   C   |  35   | African-American |   Scorpio   |
|  4   |  Paige  |   A   |  49   |  Asian-American  |    Libra    |
|  5   | stehany |   C   |  32   |     American     |    Virgo    |
|  6   |   Jon   |   A   |  47   |     Hispanic     |     Leo     |
|  7   | Charles |   B   |  40   | African-American |    Caner    |

## Conclusion:

In this post we have seen how to append the dictionary as row and column in an existing dataframe, we have used *pandas.from_dict()* function to create the dataframe from the dictionary, the *orient* parameter *index* and *columns* let you decide whether you want to add the values from dictionary as rows or columns. 

We have also seen how using pd.series() we can add the dictionary as column.