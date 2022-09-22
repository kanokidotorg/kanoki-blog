---
title: "Pandas split string into columns and split string with multiple delimiters"
date: "2022-09-22"
categories: [python, pandas]
tags: [python, pandas]
last_modified_at: 2022-09-22
permalink: python-split-dataframe-string-with-multiple-delimiters-into-columns

---

In this post we will discuss how to split a dataframe string into multiple columns and also split string with single and multiple delimiters, The most useful pandas API's for splitting the string with delimiters into multiple columns are as follows:

- <span style="color:blue">Pandas.series.str.split()</span>
- <span style="color:blue">Pandas.series.str.extract()</span>
- <span style="color:blue">Pandas.series.str.partition()</span>

We could use string or regular expression in the first two API's to split the string and return a Series or Index unless it's explicitly set <span style="color:brown">expand=True</span>, whereas for the <span style="color:blue">str.partition()</span> api we could use to split the string into two equal parts

## Split string into columns with a single delimiter
Let's start with splitting the string in the column by delimiter Comma(,)

```python
df=pd.DataFrame({
                   'storage_area':
                  [
                   '123,Green Street, Cincinnati', 
                   '9854,Oak Street', 
                   '3129,Main Street, Jackson', 
                   '4213,Bridge Ave, Trenton',
                   '09865,Rose valley, Brunswick'
                  ]
                })
df
```

We have a data frame with one (string) column and We'd like to split it into three (string) columns, with first column header as '`HouseNo'`, second column header as '`Street'` and the other as  '`City'`

|      | storage_area                 |
| ---- | ---------------------------- |
| 0    | 123,Green Street, Cincinnati |
| 1    | 9854,Oak Street              |
| 2    | 3129,Main Street, Jackson    |
| 3    | 4213,Bridge Ave, Trenton     |
| 4    | 09865,Rose valley, Brunswick |

First we will split the column into a list of strings, For a simple split over a known separator (like, splitting by dashes, or splitting by whitespace), the [`.str.split()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.str.split.html) method is enough. It operates on a column (Series) of strings, and returns a column (Series) of lists

```python
df.storage_area.str.split(',',expand=False)
```

|      |                                  |
| ---- | -------------------------------- |
| 0    | [123, Green Street,  Cincinnati] |
| 1    | [9854, Oak Street,]              |
| 2    | [3129, Main Street,  Jackson]    |
| 3    | [4213, Bridge Ave,  Trenton]     |
| 4    | [09865, Rose valley,  Brunswick] |

Getting a DataFrame out of splitting a column of strings is so useful that the <span style="color:blue">`str.split()`</span> method can do it for you with the <span style="color:brown">`expand=True`</span> parameter, It handles nicely by placing `None` in the columns for which there aren't enough "splits":

```python
df.val.str.split(',',expand=True).add_prefix('Address_')
```

|      | Address_0 | Address_1    | Address_2                           |
| ---- | --------- | ------------ | ----------------------------------- |
| 0    | 123       | Green Street | Cincinnati                          |
| 1    | 9854      | Oak Street   | <span style="color:Red">None</span> |
| 2    | 3129      | Main Street  | Jackson                             |
| 3    | 4213      | Bridge Ave   | Trenton                             |
| 4    | 09865     | Rose Valley  | Brunswick                           |



## String split multiple delimiters

We want to split a string by either a ';' or ', ' That is, it has to be either a semicolon or a comma.

```python
df=pd.DataFrame({
                 'storage_area':
                [
                 '123;Green Street, Cincinnati', 
                 '9854;Oak Street, Clifton', 
                 '3129;Main Street, Jackson', 
                 '4213;Bridge Ave, Trenton',
                 '09865;Rose valley, Brunswick'
                ]
                })
```

Let's create a dataframe with string column and components of address separated wither by semicolon(;) or a comma(,). The semicolon or comma could have a leading or trailing whitespaces.

|      | storage_area                 |
| ---- | ---------------------------- |
| 0    | 123;Green Street, Cincinnati |
| 1    | 9854;Oak Street, Clifton     |
| 2    | 3129;Main Street, Jackson    |
| 3    | 4213;Bridge Ave, Trenton     |
| 4    | 09865;Rose valley, Brunswick |

We can use str split with Regex OR condition to match multiple delimiters as shown below and set expand to True to expand the split strings into separate column.

We've added a prefix for each column

```python
df.val.str.split(',|;',expand=True).add_prefix('address_')
```



|      | Address_0 | Address_1    | Address_2  |
| ---- | --------- | ------------ | ---------- |
| 0    | 123       | Green Street | Cincinnati |
| 1    | 9854      | Oak Street   | Clifton    |
| 2    | 3129      | Main Street  | Jackson    |
| 3    | 4213      | Bridge Ave   | Trenton    |
| 4    | 09865     | Rose Valley  | Brunswick  |



## **Using Regex to split the string into columns**

Here is a unique case where we would like to split on white space such that each string split into these three columns <span style="color:blue">`HouseNo`</span>, <span style="color:blue">`Street`</span> and <span style="color:blue">`City`</span> respectively

```python
df=pd.DataFrame({
                 'storage_area':
                [
                 '123 Green Street Cincinnati', 
                 '9854 Oak Street Clifton', 
                 '3129 Main Street Jackson', 
                 '4213 Bridge Ave Trenton',
                 '09865 Rose valley Brunswick'
                ]
                })
df
```

The strings in the column are just separated by the whitespaces, we could not use the split and pass whitespace as separator because that would split into all the whitespaces, whereas we just want to split this into three columns. So what's the solution?

|      | storage_area                |
| ---- | --------------------------- |
| 0    | 123 Green Street Cincinnati |
| 1    | 9854 Oak Street Clifton     |
| 2    | 3129 Main Street Jackson    |
| 3    | 4213 Bridge Ave Trenton     |
| 4    | 09865 Rose valley Brunswick |

In such cases we could use the Regex

```python
df.storage_area.str.
extract('(?P<HouseNo>\d{1,5}) \
         ((?P<Street>.*\s)(?P<City>[a-zA-Z ]*$))')
```

We've used Named groups that will become column names in the result.

Let's understand this long Regex:

1. <span style="color:brown">`(?P\<HouseNo>\d{1,5})`</span> matches the any digit whose length is between 1 to 5
2. <span style="color:brown">`((?P\<Street>.\*\s)`</span> Matches anything else(.\*) until a whitespace in between and names this Street
3. <span style="color:brown">`(?P\<City>[a-zA-Z ]\*$)`</span> Matches any number (`*`) of lower and upper letters or spaces and names this City

|      | HouseNo | 1                       | Street       | City       |
| ---- | ------- | ----------------------- | ------------ | ---------- |
| 0    | 123     | Green Street Cincinnati | Green Street | Cincinnati |
| 1    | 9854    | Oak Street Clifton      | Oak Street   | Clifton    |
| 2    | 3129    | Main Street Jackson     | Main Street  | Jackson    |
| 3    | 4213    | Bridge Ave Trenton      | Bridge Ave   | Trenton    |
| 4    | 09865   | Rose valley Brunswick   | Rose valley  | Brunswick  |



## Split string into two columns

Pandas <span style="color:blue">str.partition()</span> function is useful when we want to split the string at the first occurrence of separator.

Excerpt from the documentation:

<span style="font-family: Open Sans; font-size: 18px;font-style: italic">*This method splits the string at the first occurrence of sep, and returns 3 elements containing the part before the separator, the separator itself, and the part after the separator. If the separator is not found, return 3 elements containing the string itself, followed by two empty strings.*</span>

```python
df=pd.DataFrame({
                  'storage_area':
                 [
                  '1234 Green Street,Cincinnati', 
                  '9854 Oak Street,Clifton', 
                  '3129 Main Street,Jackson', 
                  '4213 Bridge Ave,Trenton',
                  '0986 Rose valley,Brunswick'
                 ]
                })
df
```



|      | storage_area                 |
| ---- | ---------------------------- |
| 0    | 1234 Green Street,Cincinnati |
| 1    | 9854 Oak Street,Clifton      |
| 2    | 3129 Main Street,Jackson     |
| 3    | 4213 Bridge Ave,Trenton      |
| 4    | 0986 Rose valley,Brunswick   |

We will split the strings on whitespace and rename the two columns

```python
df['storage_area'].
str.partition(' ')[[0, 2]].
rename({0: 'HouseNo', 2: 'Address'}, axis=1)
```



|      | HouseNo | Address                 |
| ---- | ------- | ----------------------- |
| 0    | 123     | Green Street,Cincinnati |
| 1    | 9854    | Oak Street,Clifton      |
| 2    | 3129    | Main Street,Jackson     |
| 3    | 4213    | Bridge Ave,Trenton      |
| 4    | 09865   | Rose valley,Brunswick   |

