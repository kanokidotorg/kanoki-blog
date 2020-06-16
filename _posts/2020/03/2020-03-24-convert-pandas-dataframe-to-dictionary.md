---
title: "Convert Pandas dataframe to dictionary"
date: "2020-03-24"
---

In my this blog we will discover what are the different ways to convert a Dataframe into a Python Dictionary or Key/Value Pair

There are multiple ways you wanted to see the dataframe into a dictionary

We will explore and cover all the possible ways a data can be exported into a Python dictionary

Let's create a dataframe first with three columns Name, Age and City and just to keep things simpler we will have 4 rows in this Dataframe

```
import pandas as pd
df = pd.DataFrame({'Name': ['John', 'Sara','Peter','Cecilia'],
                   'Age': [38, 47,63,28],
                  'City':['Boston', 'Charlotte','London','Memphis']})
df
```

**Name**

**Age**

**City**

0

John

38

Boston

1

Sara

47

Charlotte

2

Peter

63

London

3

Cecilia

28

Memphis

## **Pandas to\_dict() function**

A simple function to convert the dataframe to dictionary. So let's convert the above dataframe to dictionary without passing any parameters

```
df.to_dict()
```

{'Name': {0: 'John', 1: 'Sara', 2: 'Peter', 3: 'Cecilia'},
 'Age': {0: 38, 1: 47, 2: 63, 3: 28},
 'City': {0: 'Boston', 1: 'Charlotte', 2: 'London', 3: 'Memphis'}}

It returns the Column header as Key and each row as value and their key as index of the datframe

If you see the Name key it has a dictionary of values where each value has row index as Key i.e. 0 as John, 1 as Sara and so on

## **Pandas Dataframe to Dictionary by Rows**

Let's change the orient of this dictionary and set it to index

```
df.to_dict('index')
```

{0: {'Name': 'John', 'Age': 38, 'City': 'Boston'},
 1: {'Name': 'Sara', 'Age': 47, 'City': 'Charlotte'},
 2: {'Name': 'Peter', 'Age': 63, 'City': 'London'},
 3: {'Name': 'Cecilia', 'Age': 28, 'City': 'Memphis'}}

Now the Dictionary key is the index of the dataframe and values are each row

The first index of dataframe is 0 which is the first key of dictionary and has a dictionary of a row as value and each value inside this dictionary has column header as Key

## **Dataframe to Dictionary with values as list**

Now change the orient to list and see what type of dictionary we get as an output

```
df.to_dict('list')
```

{'Name': \['John', 'Sara', 'John', 'Sara'\],
 'Sem': \['Sem1', 'Sem1', 'Sem2', 'Sem2'\],
 'Subject': \['Mathematics', 'Biology', 'Biology', 'Mathematics'\],
 'Grade': \['A', 'B', 'A+', 'B++'\]}

It returns Column Headers as Key and all the row as list of values

## **Pandas Dataframe to List of Dictionary**

Let's change the orient to records and check the result

```
df.to_dict('records')
```

 \[{'Name': 'John', 'Age': 38, 'City': 'Boston'},  {'Name': 'Sara', 'Age': 47, 'City': 'Charlotte'},  {'Name': 'Peter', 'Age': 63, 'City': 'London'},  {'Name': 'Cecilia', 'Age': 28, 'City': 'Memphis'}\] 

it returns the list of dictionary and each dictionary contains the individual rows

## **Dataframe to Dictionary with one Column as Key**

So we are setting the index of dataframe as Name first and then Transpose the Dataframe and convert it into a dictionary with values as list

```
df.set_index('Name').T.to_dict('list')
```

{'John': \[38, 'Boston'\],
 'Sara': \[47, 'Charlotte'\],
 'Peter': \[63, 'London'\],
 'Cecilia': \[28, 'Memphis'\]}

It returns Name as Key and the other values Age and City as list

## **Converting Timestamp data to dictionary**

Let's see how to\_dict function works with timestamp data

Let's create a simple dataframe with date and time values in it

tsmp = Timestamp("20200101")  
data = DataFrame({"A": \[tsmp, tsmp\], "B": \[tsmp, tsmp\]})  
data

**A**

**B**

0

2020-01-01

2020-01-01

1

2020-01-01

2020-01-01

```
data.to_dict(orient="records")
```

\[{'A': Timestamp('2020-01-01 00:00:00'),
  'B': Timestamp('2020-01-01 00:00:00')},
 {'A': Timestamp('2020-01-01 00:00:00'),
  'B': Timestamp('2020-01-01 00:00:00')}\]

It returns list of dictionary and each row values is a dictionary having colum label as key and timestamp object as their values

Let's take another example of dataframe with datetime object and timezone parameter info

```
data = [
            (datetime(2017, 11, 18, 21, 53, 0, 219225, tzinfo=pytz.utc),),
            (datetime(2017, 11, 18, 22, 6, 30, 61810, tzinfo=pytz.utc),),
        ]
df = DataFrame(list(data), columns=["d"])

result = df.to_dict(orient="records")
result
```

\[{'d': Timestamp('2017-11-18 21:53:00.219225+0000', tz='UTC')},
 {'d': Timestamp('2017-11-18 22:06:30.061810+0000', tz='UTC')}\]

It returns the list of dictionary with timezone info

## **Dataframe to OrderedDict and defaultdict**

### **to\_dict() Into parameter:**

You can specify the type from the collections.abc.Mapping subclass used for all Mappings in the return value

For example: the into values can be **dict, collections.defaultdict, collections.OrderedDict and collections.Counter.**

Let's take a look at these two examples here for OrderedDict and defaultdict

```
from collections import OrderedDict, defaultdict
test_data = {"A": {"1": 1, "2": 2}, "B": {"1": "1", "2": "2", "3": "3"}}
```

```
DataFrame(test_data).to_dict(into=OrderedDict)
```

OrderedDict(\[('A',
              OrderedDict(\[(0, Timestamp('2013-01-01 00:00:00')),
                           (1, Timestamp('2013-01-01 00:00:00'))\])),
             ('B',
              OrderedDict(\[(0, Timestamp('2013-01-01 00:00:00')),
                           (1, Timestamp('2013-01-01 00:00:00'))\]))\])

```
DataFrame(test_data).to_dict(into=defaultdict(list))
```

defaultdict(list,
            {'A': defaultdict(list,
                         {0: Timestamp('2013-01-01 00:00:00'),
                          1: Timestamp('2013-01-01 00:00:00')}),
             'B': defaultdict(list,
                         {0: Timestamp('2013-01-01 00:00:00'),
                          1: Timestamp('2013-01-01 00:00:00')})})

```
DataFrame(test_data).to_dict(into=dict)
```

{'A': {0: Timestamp('2013-01-01 00:00:00'),
  1: Timestamp('2013-01-01 00:00:00')},
 'B': {0: Timestamp('2013-01-01 00:00:00'),
  1: Timestamp('2013-01-01 00:00:00')}}

## **Convert a Pandas Groupby to Dictionary**

You can also group the values in a column and create the dictionary. Let's understand this with the help of this simple example

```
df = pd.DataFrame({'Serial_No': [0,1,1,1,2,2,3,3,4,5,5,5],'Segment':['A','B','C','C','B',
                                                                     'A','B','A','D','A',
                                                                    'A','C'],'Area':[23,45,64,23,64,65,23,45,23,64,23,64]})
```

**Serial\_No**

**Segment**

**Area**

0

0

A

23

1

1

B

45

2

1

C

64

3

1

C

23

4

2

B

64

5

2

A

65

6

3

B

23

7

3

A

45

8

4

D

23

9

5

A

64

10

5

A

23

11

5

C

64

We will group the above dataframe by column Serial\_No and all the values in Area column of that group will be displayed as list

```
df.groupby(['Serial_No'])['Area'].apply(list).to_dict()
```

{0: \[23\], 1: \[45, 64, 23\], 2: \[64, 65\], 3: \[23, 45\], 4: \[23\], 5: \[64, 23, 64\]}

## **Convert Dataframe to Nested Dictionary**

This is a very interesting example where we will create a nested dictionary from a dataframe

Let's create a dataframe with four columns Name, Semester, Subject and Grade

```
df = pd.DataFrame({'Name':['John','Sara','John','Sara'],'Sem':['Sem1','Sem1','Sem2','Sem2'],'Subject':['Mathematics','Biology','Biology','Mathematics'],'Grade':['A','B','A+','B++']})
 df
```

**Name**

**Sem**

**Subject**

**Grade**

0

John

Sem1

Mathematics

A

1

Sara

Sem1

Biology

B

2

John

Sem2

Biology

A+

3

Sara

Sem2

Mathematics

B++

Now we are interested to build a dictionary out of this dataframe where the key will be Name and the two Semesters (Sem 1 and Sem 2) will be nested dictionary keys and for each Semester we want to display the Grade for each Subject.

For example: John data should be shown as below

```
{
  "John": {
    "Sem1": {
      "Subject": "Mathematics",
      "Grade": "A"
    },
    "Sem2": {
      "Subject": "Biology",
      "Grade": "A+"
    }
        }
}
```

As you can see in the following code we are using a Dictionary comprehension along with groupby to achieve this.

We have set the index to Name and Sem which are the Keys of each dictionary and then grouping this data by Name

And iterating this groupy object inside the dictionary comprehension to get the desired dictionary format

```
ppdict = {n: grp.loc[n].to_dict('index')
 for n, grp in df.set_index(['Name', 'Sem']).groupby(level='Name')}
print (json.dumps(ppdict, indent=2))
```

{
  "John": {
    "Sem1": {
      "Subject": "Mathematics",
      "Grade": "A"
    },
    "Sem2": {
      "Subject": "Biology",
      "Grade": "A+"
    }
  },
  "Sara": {
    "Sem1": {
      "Subject": "Biology",
      "Grade": "B"
    },
    "Sem2": {
      "Subject": "Mathematics",
      "Grade": "B++"
    }
  }
}

## **Conclusion**

So just to summarize our key learning in this post, here are some of the main points that we touched upon:

- How to convert a dataframe into a dictionary using to\_dict() function
- Using the oriented parameter to customize the result of our dictionary
- into parameter can be used to specify the return type as defaultdict, Ordereddict and Counter
- How a data with timestamp and datetime values can be converted into a dictionary
- Using groupby to group values in one column and converting the values of another column as list and finally converting it into a dictionary
- Finally how to create a nested dictionary from your dataframe using groupby and dictionary comprehension

* * *
