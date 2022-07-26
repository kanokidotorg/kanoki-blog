---
title: "Pandas sort dataframe by month name"
date: "2022-06-27"
categories: [ pandas, python]
tags: [ pandas, python]

---

In this post we will see how to sort a dataframe by month name(string) column. It's bit straight forward to sort a number or string alphabetically using ***sort_values()*** function in pandas but in this post we are gonna talk about sorting a string column in specific order such as month of year.

We will use ***key*** parameter in *sort_values()* function to sort the rows by month name in our dataframe. There are three different ways shown in this post to use *key* argument for sorting.

Let's get started, First we will create our dataframe to work upon:

### Create Dataframe

```python
import pandas as pd
data = {
    'month': ['Apr', 'Jan', 'Jul', 'Oct', 'Nov', 'Sep', 'Dec']
   ,'sales': [6100, 3029, 6094, 4932, 3559, 4648, 5615]
}
df = pd.DataFrame(data)
df
```
There are two columns in this dataframe month and sales. The column month is not in order and the goal is to sort the dataframe rows by the column month.

|      |     month    |   sales   |
| ----------- | :-----------: |-----------|
| 0      | Apr          |  6100 |
| 1      | Jan           |  3029 |
| 2      | Jul           |  6094 |
| 3      | Oct           |  4932 |
| 4      | Nov           |  3559 |
| 5      | Sep           |  4648 |
| 6	      | Dec           |  5615 |

### Sort values by month using sort_values() and creating a month dictionary

The key argument in ***sort_values()*** function work similar to the key in built-in ***soreted()*** function. It should expect a Series and return a Series with the same shape as the input and will be applied to each column independently

We will first create a month dictionary to map the month name with their respective integer values

```python
month_dict = {'Jan':1,'Feb':2,'Mar':3, 'Apr':4, 'May':5, 'Jun':6, 'Jul':7, 'Aug':8, 'Sep':9, 'Oct':10, 'Nov':11, 'Dec':12}
```
Next we will use *key* argument in **sort_values()** function to map these month names to their integer value and sort by the integers as shown below

```python
df.sort_values('month', key = lambda x : x.apply (lambda x : month_dict[x]))
```

|      |     month    |   sales   |
| ----------- | :-----------: |-----------|
| 0      | Jan          |  3029 |
| 1      | Apr           |  6100 |
| 2      | Jul           |  6094 |
| 3      | Sep           |  4648 |
| 4      | Oct           |  4932 |
| 5      | Nov        | 3559 |
| 6      | Dec           |  5615 |

So you can see the result is a sorted dataframe rows by month name. Isn't the *key* argument is awesome, we sorted the dataframe by mapping the month by their int values on the fly.

### Sort values by month using sort_values() and converting month to datetime

In this section, we will convert the month column to pandas datetime and then find the integer value of the month, It's similar to what we did above but without a dictionary this time.

let's see the integer output of the month column by converting to datetime and finding the integer value using the dt Accessor object for datetimelike properties of the Series values

```python
pd.to_datetime(df.month, format='%b').dt.month
```


|      |     month    |
| ----------- | :-----------: |
| 0      | 4          |
| 1      | 1           |
| 2      | 7           |
| 3      | 10           |
| 4      | 11           |
| 5      | 9        |
| 6      | 12           |

If you will see carefully this is the integer value of month column of original dataframe in the same order

Next we will pass this datetime object month in *key* parameter of ***sort_values()*** to sort the dataframe by month name

```python
df.sort_values('month', key = lambda x : pd.to_datetime(x, format='%b').dt.month)
```
**Output**

|      |     month    |   sales   |
| ----------- | :-----------: |-----------|
| 0      | Jan          |  3029 |
| 1      | Apr           |  6100 |
| 2      | Jul           |  6094 |
| 3      | Sep           |  4648 |
| 4      | Oct           |  4932 |
| 5      | Nov        | 3559 |
| 6      | Dec           |  5615 |

The result is same as the above section, all the rows of dataframe are sorted by the month 

### Sort values by month using sort_values() and converting month to Categorical

In this part, we will convert the month to Categorical which means it can only take on only a limited, and usually fixed, number of possible values (categories) define unique categories for this categorical.

Let's first define the categories for this categorical which will be months of a year as shown below

```python
months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", 
          "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
```

Next we will sort the dataframe by month column and key will be the Categorical month 

```python
df.sort_values('month', key = lambda x : pd.Categorical(x, categories=months, ordered=True))
```

**Output**

|      |     month    |   sales   |
| ----------- | :-----------: |-----------|
| 0      | Jan          |  3029 |
| 1      | Apr           |  6100 |
| 2      | Jul           |  6094 |
| 3      | Sep           |  4648 |
| 4      | Oct           |  4932 |
| 5      | Nov        | 3559 |
| 6      | Dec           |  5615 |

### Conclusion:

Here are the three different ways in which key argument in sort_values() can be used to sort dataframe by column month name

- Sort by month by creating a dictionary of month values and it's corresponding integer values
- Sort using sort_values() by converting month column to datetime and accessing the integer value of month by dt accessor
- Sort values by converting month to Categorical and define the month of a year as categories