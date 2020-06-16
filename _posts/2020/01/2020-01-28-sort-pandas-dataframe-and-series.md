---
title: "Sort Pandas Dataframe and Series"
date: "2020-01-28"
categories: [ Pandas, Python ]
tags: [ Pandas, Python ]
---

Sorting a dataframe by row and column values or by index is easy a task if you know how to do it using the pandas and numpy built-in functions

However sometimes you may find it confusing on how to sort values by two columns, a list of values or reset the index after sorting.

In this post we will learn sorting a dataframe and Series using the following functions

> **a) sort\_values
> b) sort\_index
> c) Categorical Series
> d) numpy sort and argsort
> e) Reindex
> f) And Sorted() function**

Let's create a dataframe of 11 counties with their CO2 emission and population and a column for the continent they belong to

```
import numpy as np
import pandas as pd
df = pd.DataFrame({
    'Country':['USA','China','India','Russia','Switzerland','Japan','Sweden','Singapore','South Korea','UK','Australia'],
    'Co2_emission':[5107.393,10877.218,2454.774,1764.866,39.738,1320.776,np.NaN,55.018,673.324,379.150,764.866],
    'Population_million':[329,1433,1366,145,8.5,126,10,5.8,51,67,np.NaN],
    'Continent': ['NA','Asia','Asia','EU','EU','Asia','EU','Asia','Asia','EU','AU']
                 })
```

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

0

USA

5107.393

329.0

NA

1

China

10877.218

1433.0

Asia

2

India

2454.774

1366.0

Asia

3

Russia

1764.866

145.0

EU

4

Switzerland

39.738

8.5

EU

5

Japan

1320.776

126.0

Asia

6

Sweden

NaN

10.0

EU

7

Singapore

55.018

5.8

Asia

8

South Korea

673.324

51.0

Asia

9

UK

379.150

67.0

EU

10

Australia

764.866

NaN

AU

## **Sort dataframe by column Values**

Here we are sorting the dataframe by column values i.e. population\_in\_million

sort\_values functions sorts the dataframe by the column values provided as the first argument and setting ascending as True/False will display the results in that order

```
df.sort_values('Population_million',ascending = False)
```

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

1

China

10877.218

1433.0

Asia

2

India

2454.774

1366.0

Asia

0

USA

5107.393

329.0

NA

3

Russia

1764.866

145.0

EU

5

Japan

1320.776

126.0

Asia

9

UK

379.150

67.0

EU

8

South Korea

673.324

51.0

Asia

6

Sweden

NaN

10.0

EU

4

Switzerland

39.738

8.5

EU

7

Singapore

55.018

5.8

Asia

10

Australia

764.866

NaN

AU

## **Sort Dataframe with NaN on Top or Bottom**

The column contains the NaN value and you can choose to display the NaN values either on Top or Bottom row of the sorted dataframe

na\_position parameters you can set as first or last to push those values on the top or the bottom of dataframe

```
df.sort_values('Population_million',ascending = False,na_position = 'first')
```

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

10

Australia

764.866

NaN

AU

1

China

10877.218

1433.0

Asia

2

India

2454.774

1366.0

Asia

0

USA

5107.393

329.0

NA

3

Russia

1764.866

145.0

EU

5

Japan

1320.776

126.0

Asia

9

UK

379.150

67.0

EU

8

South Korea

673.324

51.0

Asia

6

Sweden

NaN

10.0

EU

4

Switzerland

39.738

8.5

EU

7

Singapore

55.018

5.8

Asia

## **Sort dataframe rows by multiple columns**

You can also provide the list of columns to be sorted and their order also a list of boolean

Here we are sorting the dataframe first by column CO2\_emission and then by Population\_million and we want the order of the CO2\_emission in ascending and population\_million in Descending

```
df.sort_values(by = ['Co2_emission','Population_million'],ascending = [True,False])
```

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

4

Switzerland

39.738

8.5

EU

7

Singapore

55.018

5.8

Asia

9

UK

379.150

67.0

EU

8

South Korea

673.324

51.0

Asia

10

Australia

764.866

NaN

AU

5

Japan

1320.776

126.0

Asia

3

Russia

1764.866

145.0

EU

2

India

2454.774

1366.0

Asia

0

USA

5107.393

329.0

NA

1

China

10877.218

1433.0

Asia

6

Sweden

NaN

10.0

EU

## **Sort text column by Alphabetical order**

if there is a text column and you want to sort it just alphabetically then applying sort\_values on that column will do so

```
df.sort_values(by='Country')
```

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

10

Australia

764.866

NaN

AU

1

China

10877.218

1433.0

Asia

2

India

2454.774

1366.0

Asia

5

Japan

1320.776

126.0

Asia

3

Russia

1764.866

145.0

EU

7

Singapore

55.018

5.8

Asia

8

South Korea

673.324

51.0

Asia

6

Sweden

NaN

10.0

EU

4

Switzerland

39.738

8.5

EU

9

UK

379.150

67.0

EU

0

USA

5107.393

329.0

NA

## **Sort by Custom list or Dictionary using Categorical Series**

Here we wanted to sort the dataframe by the continent column but in a particular custom order and not alphabetically

That custom order can be a list or the keys of a dictionary

\[ "NA", "Asia", "AU", "EU"\])

or

{"NA": 0, "Asia": 1, "AU": 2, "EU": 3}

Pandas has an excellent feature called [Categorical Series](http://pandas.pydata.org/pandas-docs/stable/categorical.html) using which we can sort the column values in the pre-defined order

First make the Continent column a categorical and specify the ordering to use by passing a list value as the categories argument

df \[ 'Continent'\] = pd.Categorical(df\['Continent'\], categories=\["NA", "Asia", "AU", "EU"\])

Finally, sort the values by Continent column and it will order the rows as per the custom list passed in the categories column

```
df.sort_values(by='Continent')
```

Country

**Co2\_emission**

**Population\_million**

**Continent**

USA

5107.393

329.0

NA

China

10877.218

1433.0

Asia

India

2454.774

1366.0

Asia

Japan

1320.776

126.0

Asia

Singapore

55.018

5.8

Asia

South Korea

673.324

51.0

Asia

Australia

764.866

NaN

AU

Russia

1764.866

145.0

EU

Switzerland

39.738

8.5

EU

Sweden

NaN

10.0

EU

UK

379.150

67.0

EU

## **Sort dataframe based on a custom list**

It's not necessary that everytime you use the sort\_values function for sorting

Here is a unique case if you have list of countries and want to arrange the dataframe rows based on the elements inside this list

First set the index of the dataframe to Country column and then pass the list as argument to the reindex function and that will Conform DataFrame to new index as per the list

```
reorderlist = ['China','Russia','Australia','India','Japan','Singapore','South Korea','UK','USA','Sweden','Switzerland']
df.set_index('Country',inplace=True)
df.reindex(reorderlist)
```

Country

**Co2\_emission**

**Population\_million**

**Continent**

China

10877.218

1433.0

Asia

Russia

1764.866

145.0

EU

Australia

764.866

NaN

AU

India

2454.774

1366.0

Asia

Japan

1320.776

126.0

Asia

Singapore

55.018

5.8

Asia

South Korea

673.324

51.0

Asia

UK

379.150

67.0

EU

USA

5107.393

329.0

NA

Sweden

NaN

10.0

EU

Switzerland

39.738

8.5

EU

## **reset index after sorting dataframe**

When you sort the dataframe the index are moved up and down and to re-arrange the index you can use reset\_index function and set drop as True to drop the index column which will be inserted as a new column

```
df.sort_values('Population_million',ascending = False).reset_index(drop=True)
```

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

0

China

10877.218

1433.0

Asia

1

India

2454.774

1366.0

Asia

2

USA

5107.393

329.0

NA

3

Russia

1764.866

145.0

EU

4

Japan

1320.776

126.0

Asia

5

UK

379.150

67.0

EU

6

South Korea

673.324

51.0

Asia

7

Sweden

NaN

10.0

EU

8

Switzerland

39.738

8.5

EU

9

Singapore

55.018

5.8

Asia

10

Australia

764.866

NaN

AU

## **Sort dataframe by date column**

sort\_values functions let you sort the dataframe by dates also

Let's create a column with date range

```
df['Date']=pd.date_range(start='1/12/2019', end='1/22/2019', freq='D')
```

Sort the column by Date in descending order

```
df.sort_values('Date',ascending=False,inplace=True)
```

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

**Date**

10

Australia

764.866

NaN

AU

2019-01-22

9

UK

379.150

67.0

EU

2019-01-21

8

South Korea

673.324

51.0

Asia

2019-01-20

7

Singapore

55.018

5.8

Asia

2019-01-19

6

Sweden

NaN

10.0

EU

2019-01-18

5

Japan

1320.776

126.0

Asia

2019-01-17

4

Switzerland

39.738

8.5

EU

2019-01-16

3

Russia

1764.866

145.0

EU

2019-01-15

2

India

2454.774

1366.0

Asia

2019-01-14

1

China

10877.218

1433.0

Asia

2019-01-13

0

USA

5107.393

329.0

NA

2019-01-12

## **Sort dataframe by datetime index using sort\_index**

You can also sort the df by datetime index using the sort\_index function

First set the date column created above as an index and then sort the index

```
df.set_index('Date').sort_index()
```

Date

**Country**

**Co2\_emission**

**Population\_million**

**Continent**

2019-01-12

USA

5107.393

329.0

NA

2019-01-13

China

10877.218

1433.0

Asia

2019-01-14

India

2454.774

1366.0

Asia

2019-01-15

Russia

1764.866

145.0

EU

2019-01-16

Switzerland

39.738

8.5

EU

2019-01-17

Japan

1320.776

126.0

Asia

2019-01-18

Sweden

NaN

10.0

EU

2019-01-19

Singapore

55.018

5.8

Asia

2019-01-20

South Korea

673.324

51.0

Asia

2019-01-21

UK

379.150

67.0

EU

2019-01-22

Australia

764.866

NaN

AU

## **Sort dataframe by row values**

Here we will see how to sort the dataframe by a specific row values

Let's create a dataframe of numbers first

```
df = pd.DataFrame(data={'x':[28,10,90], 'y':[45,58,67],'z':[19,82,37]}, index=['a', 'b', 'c'])
```

x

y

z

a

28

45

19

b

10

58

82

c

90

67

37

**Sort row values using sort\_values**

We are sorting the above dataframe based on the values in row a and by default it is ascending order

```
df.sort_values(by='a', axis=1)
```

z

x

y

a

19

28

45

b

82

10

58

c

37

90

67

Note: In all the below cases the output will be same as the above table

**Sort row values using numpy**

We can also use numpy sort and argsort to achieve the same result as above

```
df.iloc[:, np.argsort(df.loc['a'])]
```

or

```
 df.apply(np.sort, axis = 1)
```

Alternatively, if you want to reverse the order then

```
df[['x', 'y', 'z']] = np.sort(df)[:, ::-1]
```

**Sort row values using reindex**

We can also use reindex and sorted function with a lambda as a key to sort the dataframe based on values in row a

```
df.reindex(sorted(df.columns, key=lambda x: df[x]['a']), axis=1)
```

## **Conclusion:**

So we have reached to the end of this blog post and just wanted to highlight few key points that we have learn in this blog post

1. Sorting of the dataframe by single, multiple column values and arranging the sorted columns in ascending and descending order
2. How to use sort\_values functions and the arguments like na\_position, ascending etc.
3. Sorting columns based on a custom list or dictionary and using Pandas Categorical Series and reindex
4. How to Sort with date column and datetime index using sort\_index
5. Sorting based on a specific row values by different methods like numpy argsort, sort\_values and reindex

if there is any additional function which you know can be used to sort the rows and column values in a dataframe then please leave your comments in the comment section below

* * *
