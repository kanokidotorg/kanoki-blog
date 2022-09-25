---
title: "How to Rename Columns in Pandas"
date: "2019-03-23"
categories: [ Data Science, Pandas, Python ]
tags: [ DataScience, Pandas, Python ]
last_modified_at: 2022-09-11
Permalink: rename-columns-in-pandas
---

Pandas has two ways to rename their Dataframe columns:

- first using the df.rename() function and 
- second by using df.columns, which is the list representation of all the columns in dataframe. 

Let's Start with creating a dataframe and then we will see different ways to rename the columns of this dataframe and eventually how to reorder the column labels.

## **Create Dataframe**

Let's create a dataframe of Students details and each of the column has a special character $ prefixed to it's columns name and we  want to strip that special character from the column names

```python
df = pd.DataFrame({'$name':['Allan','Mike','Brenda','Holy'], '$Age': [30,20,25,18],'$Zodiac':['Aries','Leo','Virgo','Libra'],'$Grade':['A','AB','B','AA'],'$City':['Aura','Somerville','Boon','Gannon']})
```

|      | $Name  | $Age | $Zodiac | $Grade |   $City    |
| ---- | :----: | :--: | :-----: | :----: | :--------: |
| 0    | Allan  |  30  |  Aries  |   A    |    Aura    |
| 1    |  Mike  |  20  |   Leo   |   AB   | Somerville |
| 2    | Brenda |  25  |  Virgo  |   B    |    Boon    |
| 3    |  Holy  |  18  |  Libra  |   AA   |   Gannon   |



## **Pandas Rename Multiple Columns**

pandas df.rename() api is used to alter the axes labels, It takes a dict-like or function transformations to apply to that axis’ values

Let's rename all the column by stripping the character $ from it's label, First we will create a dictionary of old and new column labels as shown below

The existing labels are the keys and the new column labels are the values in the dictionary.

```python
column_labels = {'$Name': 'Name',
                '$Age': 'Age',
                '$Zodiac': 'Zodiac',
                '$Grade': 'Grade',
                '$City': 'City'}    
```

Next we will pass this dictionary to df.rename() to rename the column labels of the dataframe

```python
df.rename(columns=column_labels,inplace=True)
```

Alternatively, we can also replace the $ character for all the column labels using the df.columns as shown below

```python
df.columns=[it.replace('$','') for it in df.columns]
df
```

|      |  Name  | Age  | Zodiac | Grade |    City    |
| :--: | :----: | :--: | :----: | :---: | :--------: |
|  0   | Allan  |  30  | Aries  |   A   |    Aura    |
|  1   |  Mike  |  20  |  Leo   |  AB   | Somerville |
|  2   | Brenda |  25  | Virgo  |   B   |    Boon    |
|  3   |  Holy  |  18  | Libra  |  AA   |   Gannon   |



## **Pandas set column names** 

We will replace the df.columns list value with the new list values which is created by stripping the $ character from the column labels 

Let's create this new list using the str.replace() function  and  after that assign this new list to df.columns 

```python
df.columns = df.columns.str.replace('$','')
```

Let's check the new column labels in df.columns list

```python
df.columns
Out: Index(['Age', 'City', 'Grade', 'Zodiac', 'name'], dtype='object')
```

The new column values doesn't have those $ character in the column name

|      |  Name  | Age  | Zodiac | Grade |    City    |
| :--: | :----: | :--: | :----: | :---: | :--------: |
|  0   | Allan  |  30  | Aries  |   A   |    Aura    |
|  1   |  Mike  |  20  |  Leo   |  AB   | Somerville |
|  2   | Brenda |  25  | Virgo  |   B   |    Boon    |
|  3   |  Holy  |  18  | Libra  |  AA   |   Gannon   |

## **Pandas rename column with a list**

We can  replace the existing column labels with the new column labels in the list by assigning the value to df.columns as shown below

Here we changed the column labels to it's german equivalent

```python
df.columns = ['Nennen','Das Alter','Tierkreis','Klasse','Stadt']
df
```

|      | Nennen | Das Alter | Tierkreis | Klasse |   Stadt    |
| :--: | :----: | :-------: | :-------: | :----: | :--------: |
|  0   | Allan  |    30     |   Aries   |   A    |    Aura    |
|  1   |  Mike  |    20     |    Leo    |   AB   | Somerville |
|  2   | Brenda |    25     |   Virgo   |   B    |    Boon    |
|  3   |  Holy  |    18     |   Libra   |   AA   |   Gannon   |

## **Pandas Rename Columns by Index**

We can rename the columns by their Index as well, we will first create a dictionary as shown below, where the keys of dictionary will be selected by the index of the column

Here we want to rename the first and third column in the dataframe to col1 & col3 respectively

```python
column_labels_index = {
                df.columns[0]: 'Col1',
                df.columns[2]: 'Col3'
                }  
```

Next we will  pass the above dictionary to df.rename() function

```python
df.rename(columns=column_labels_index, inplace=True)
```



|      |  Col1  | Age  | Col3  | Grade |    City    |
| :--: | :----: | :--: | :---: | :---: | :--------: |
|  0   | Allan  |  30  | Aries |   A   |    Aura    |
|  1   |  Mike  |  20  |  Leo  |  AB   | Somerville |
|  2   | Brenda |  25  | Virgo |   B   |    Boon    |
|  3   |  Holy  |  18  | Libra |  AA   |   Gannon   |



## **Reorder Columns in Pandas using df.columns**

We want to re-order the column by moving the Name column to the last and Age column to the first.

Let's see how we can do this by simply assigning the new values in a list to df.columns

```python
# Re-order Columns
df.columns = ['Age','Zodiac','Grade','City','Name']
df
```

|      | Age  | Zodiac | Grade |    City    | Name   |
| :--: | :--: | :----: | :---: | :--------: | ------ |
|  0   |  30  | Aries  |   A   |    Aura    | Allan  |
|  1   |  20  |  Leo   |  AB   | Somerville | Mike   |
|  2   |  25  | Virgo  |   B   |    Boon    | Brenda |
|  3   |  18  | Libra  |  AA   |   Gannon   | Holy   |



