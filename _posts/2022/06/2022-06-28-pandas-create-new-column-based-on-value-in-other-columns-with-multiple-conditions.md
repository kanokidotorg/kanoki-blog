---
title: "Pandas create new column based on value in other column with multiple conditions"
date: "2022-06-28"
categories: [ pandas, python]
tags: [ pandas, python]

---

In this post we will see how to create a new column based on values in other columns with multiple if/else-if conditions. It's bit straight forward to create a new column with just a simple if-else condition but in this post we will focus on multiple if/elseif conditions that returns different values for each condition.

We will use three different ways as shown in this post to create the new row "Grade" in our dataframe. 

Let's get started, First we will create our dataframe to work upon:

### Create Dataframe

```python
import pandas as pd
import numpy as np

data = {'student':['A','B','C','D','E'],
    'IQ': [124, 98,119,136,88],
    'English' : [74,49,65,18,149],
    'Mathematics' : [29,38,73,43,34],
    'Physics' : [54,67,49,172,99]}

df = pd.DataFrame(data)
df
```
There are 5 columns in this dataframe student, IQ, and their scores in English, Mathematics and Physics. The goal is to compute the grade of each student using their score in each subject and IQ level.

|      |     student    |   IQ   | English| Mathematics | Physics|
| ----------- | :-----------: | :-----------:| :-----------: | :-----------:|:-----------:|
| 0      | A          |  124 | 74 | 29 | 54 |
| 1      | B          |  98 | 49 | 38 | 67 |
| 2      | C          |  119 | 65 | 73 | 49 |
| 3      | D          |  136 | 18 | 43 | 172 |
| 4      | E          |  88 | 149 | 34 | 99 |

### Create column using a custom function applied across all the rows

So we wanted to compute the Grade column using the values from other columns like scores in respective subjects and IQ level.

We will first create a function that returns the grade based on the multiple conditions 

```python
def compute_grade(df):    
    if (df['English'] <= df['IQ']) & (df['Mathematics'] > df['Physics']):
        return 'A'
    elif df['Physics'] <= df['IQ'] | (df['Physics']>50) & ((df['Mathematics'] >  df['IQ']) | (df['English']>30)):
        return 'A+'
    elif (df['Mathematics'] <= df['IQ']) & (df['English'] < 30):
        return 'B'
    elif (df['Physics'] > 60) & ((df['Mathematics'] | df['English'])>50):
        return "C"	
```

So we create this function compute_grade() for computing the grades for each student and you can see the conditions for each of the Grade under the if clause in each line and the return value of each if-condition is the Grade

Next we will use pandas ***apply*** function to Apply a function along an axis of the DataFrame, in this case axis=1 i.e. rows

```python
df['Grade'] = df.apply(compute_grade, axis = 1)
```

Or, we can also use pandas ***assign*** function to Assign new columns to a DataFrame and it Returns a new object with all original columns in addition to new ones. Existing columns that are re-assigned will be overwritten

```python
df = df.assign(Grade=df.apply(compute_grade, axis=1))
```

|      |     student    |   IQ   | English| Mathematics | Physics| Grade |
| ----------- | :-----------: | :-----------:| :-----------: | :-----------:|:-----------:| :-----------:|
| 0      | A          |  124 | 74 | 29 | 54 | A+ |
| 1      | B          |  98 | 49 | 38 | 67 | A+ |
| 2      | C          |  119 | 65 | 73 | 49 | A |
| 3      | D          |  136 | 18 | 43 | 172 | B |
| 4      | E          |  88 | 149 | 34 | 99 | C |

### Create column using numpy select

Alternatively and one of the best way to create a new column with multiple condition is using numpy.select() function. It takes the following three parameters and Return an array drawn from elements in choicelist, depending on conditions

- **condlist**
 - 	The list of conditions which determine from which array in choicelist the output elements are taken
- **choicelist**
 - 	The list of arrays from which the output elements are taken. It has to be of the same length as condlist
- **default**
 - 	The element inserted in output when all conditions evaluate to False

 We will create an array of conditions and it's respective choices in two separate list

 ```python
 conditions = [
    ((df['English'] <= df['IQ']) & (df['Mathematics'] > df['Physics'])),
    ((df['Physics'] <= df['IQ'])& (df['Physics']>50) & ((df['Mathematics'] >  df['IQ']) | (df['English']>30))),
    ((df['Mathematics'] <= df['IQ']) & (df['English'] < 30)),
    ((df['Physics'] > 60) & ((df['Mathematics'] | df['English'])>50))
]
choices = ['A','A+','B', 'C']

 ```

 The conditions are same as it was defined in the above compute_grade() function and the choices is a list of all the return values from the function. Note the length of conditions and choices are equal.

 Next we will compute the grade and create a new column using the conditions and it's choices

 ```python
 df['Grade'] = np.select(conditions, choices, default='NA')
 ```

|      |     student    |   IQ   | English| Mathematics | Physics| Grade |
| ----------- | :-----------: | :-----------:| :-----------: | :-----------:|:-----------:| :-----------:|
| 0      | A          |  124 | 74 | 29 | 54 | A+ |
| 1      | B          |  98 | 49 | 38 | 67 | A+ |
| 2      | C          |  119 | 65 | 73 | 49 | A |
| 3      | D          |  136 | 18 | 43 | 172 | B |
| 4      | E          |  88 | 149 | 34 | 99 | C |

You can see the output is same as above, however this is more elegant and cleaner way to create columns based on conditions.

### Create column using loc

dataframe.loc can be used to create new columns with conditions as well, Let's define a function with all the conditions in a dataframe.loc

We created a boolean mask with all our conditions and pass this mask in .loc to update the column with desired value.

There are 4 conditions and fifth is default to fill all the rows where condition are not satisfied with a default value 'C'

```python
def compute_grade(df):
    df.loc[(df['English'] <= df['IQ']) & (df['Mathematics'] > df['Physics']),'Grade'] = 'A'
    df.loc[(df['Physics'] <= df['IQ']) & (df['Physics']>50) & ((df['Mathematics'] < df['IQ']) | (df['English']>30)),'Grade'] = 'A+'
    df.loc[((df['Mathematics'] <= df['IQ']) & (df['English'] < 30)),'Grade'] = 'B'
    df.loc[(df['Physics'] > 60) & ((df['Mathematics']>50) | (df['English']>50)),'Grade'] = 'C'
    df['Grade'].fillna('C', inplace=True)
```
Now execute this function by passing the dataframe and it returns the dataframe with new column Grade

```python
compute_grade(df)
```




|      |     student    |   IQ   | English| Mathematics | Physics| Grade |
| ----------- | :-----------: | :-----------:| :-----------: | :-----------:|:-----------:| :-----------:|
| 0      | A          |  124 | 74 | 29 | 54 | A+ |
| 1      | B          |  98 | 49 | 38 | 67 | A+ |
| 2      | C          |  119 | 65 | 73 | 49 | A |
| 3      | D          |  136 | 18 | 43 | 172 | B |
| 4      | E          |  88 | 149 | 34 | 99 | C |

### Conclusion:

Here are the three different ways in which a new column could be created from other columns based on multiple conditions

- Create a new column using a custom function with condtions and return value to apply across all the rows of dataframe
- Use numpy select with list of conditions and their respective choices as parameters to create a new column
- Create conditions as boolean mask to pass in .loc to update the new column value
