---
title: "Pandas extract numbers and floats  from string"
date: "2022-08-24"
categories: [ pandas, python]
tags: [ pandas, python]

---

We want to extract the numbers, float and multiple numbers from a string column in a dataframe.

Here are the steps that we will follow for extracting the numbers and floats from the string column

1. Create a dataframe with string column that contains alpha-numeric characters in it
2. **Pandas.Series.str.extract()** function will extract only the first number or floats based on the regex pattern passed to it
3. **Pandas.Series.str.extractall()** function will extract all the integers or floats or both from the string based on the regex pattern passed to it
4. Also, we can use **Pandas.Series.str.findall()** function to extract all the numbers and floats values from the string

Let's get started, we will first create a test dataframes(df) to work upon

## Create a Dataframe


This dataframe has a Alpha_Numeric column with string values containing numbers or floats in it


```python
import pandas as pd
import numpy as np

df = pd.DataFrame({'Alpha-Numeric':['1a',np.nan,'10a','100.0b','0b', '3.2 8,abc9.67'],
                   })
df
```

|      |  Alpha-Numeric   |
| :--: | :--------------: |
|  0   |        1a        |
|  1   |       NaN        |
|  2   |      10,00a      |
|  3   |      100.0b      |
|  4   | 0 4.095 8holland |
|  5   |  3.2 8,abc9.67   |

## Extract Number from Strings

We have used **Pandas.Series.str.extract()** to extract groups from the first match of regular expression pat(digits in this case), It takes following three parameters:

- **Pat:** Regular Expression pattern
- **Flags:** Flags from the `re` module, e.g. `re.IGNORECASE`, default is 0, which means no flags
- **expand:** if True returns a dataframe with one column per capture group

It returns a DataFrame with one row for each subject string, and one column for each group

```python
df['Extracted_Digit']=df.Alpha_Numeric.str.extract('(^\d*)')
df
```

|      |  Alpha_Numeric   | Extracted_Digit |
| :--: | :--------------: | :-------------: |
|  0   |        1a        |        1        |
|  1   |       NaN        |       NaN       |
|  2   |      10,00a      |       10        |
|  3   |      100.0b      |       100       |
|  4   | 0 4.095 8holland |        0        |
|  5   |  3.2 8,abc9.67   |        3        |

The Extracted_Digit column has all the integers from the String in column Alpha_Numeric, Here is some explanation for the strings which has floats or special characters in the string:

- Row Index# 2, The extracted digit is 10 and it ignores anything followed by comma(,)
- Row Index# 3, The extracted digit is 100 and it does not captures the decimal values
- Row Index# 4, Only the first integer is extracted i.e. 0
- Row Index# 5, Only the first integral part i.e. 3 is extracted from 3.2 and the decimal value is ignored

## Extract Float from String

To extract the floats from strings, We could use **Pandas.Series.str.extract()** and just need to change the regular expression to extract the float values 

```python
df['Extracted_Float']=df.Alpha_Numeric.str.extract(r"(\d+\.\d+)")
df
```

It extracted the float values at row index# 3, 4 and 5 only, which have float values in the string and for the rest of the rows the value is NaN

|      |  Alpha_Numeric   | Extracted_Float |
| :--: | :--------------: | :-------------: |
|  0   |        1a        |       NaN       |
|  1   |       NaN        |       NaN       |
|  2   |      10,00a      |       NaN       |
|  3   |      100.0b      |      100.0      |
|  4   | 0 4.095 8holland |      4.095      |
|  5   |  3.2 8,abc9.67   |       3.2       |

We could also extract both Integer and float values by changing the regular expression as shown below

```python
df['Extracted_Int_Float']=df.Alpha_Numeric.str.extract('([-+]?\d*\.?\d+)')
```

The Extracted_Int_Float column has both the integral part and decimal values from the string, It extracts only the first found number in the String value.

Row Index#5 has extracted the first encountered float value i.e. 3.2 and row index#3 has extracted the float value 100.0(decimal value)

|      |  Alpha_Numeric   | Extracted_Int_Float |
| :--: | :--------------: | :-----------------: |
|  0   |        1a        |          1          |
|  1   |       NaN        |         NaN         |
|  2   |      10,00a      |         10          |
|  3   |      100.0b      |        100.0        |
|  4   | 0 4.095 8holland |          0          |
|  5   |  3.2 8,abc9.67   |         3.2         |



## Extract multiple numbers from String

We will first use **Pandas.Series.str.findall()** to find all occurrences of pattern or regular expression in the Series/Index, It is similar to applying [`re.findall()`](https://docs.python.org/3/library/re.html#re.findall) to all the elements in the Series/Index, It takes following two parameters:

- **Pat:** Regular Expression pattern
- **Flags:** Flags from the `re` module, e.g. `re.IGNORECASE`, default is 0, which means no flags

It returns all non-overlapping matches of pattern or regular expression in each string of this Series/Index



```python
df['Extract_All'] = df.Alpha_Numeric.str.findall(r'(\d+(?:\.\d+)?)')
df
```



|      |  Alpha_Numeric   |  Extract_All   |
| :--: | :--------------: | :------------: |
|  0   |        1a        |      [1]       |
|  1   |       NaN        |      NaN       |
|  2   |      10,00a      |    [10,00]     |
|  3   |      100.0b      |    [100.0]     |
|  4   | 0 4.095 8holland | [0, 4.095, 8]  |
|  5   |  3.2 8,abc9.67   | [3.2, 8, 9.67] |

The Extract_All columns has all the numbers and floats in a list format.

We could also explode the dataframe above to transform each element of a list-like to a row, replicating index values using **pandas.dataframe.explode()**

```python
df.explode('Extract_All')
```



|      |   Alpha_Numeric   | Extract_All |
| :--: | :---------------: | :---------: |
|  0   |        1a         |     [1]     |
|  1   |        NaN        |     NaN     |
|  2   |      10,00a       |     10      |
|  2   |      10,00a       |     00      |
|  3   |      100.0b       |    100.0    |
|  4   | 0 4.095b 8holland |      0      |
|  4   | 0 4.095b 8holland |    4.095    |
|  4   | 0 4.095b 8holland |      8      |
|  5   |   3.2 8,abc9.67   |     3.2     |
|  5   |   3.2 8,abc9.67   |      8      |
|  5   |   3.2 8,abc9.67   |    9.67     |

Alternatively, We could also use **Pandas.Series.str.extractall()** to extract multiple numbers from string, It extract capture groups in the regex pat as columns in DataFrame.

It takes the same parameters as str.findall() and returns a DataFrame  with one row for each match, and one column for each group. Its rows have a `MultiIndex` with first levels that come from the subject `Series`. The last level is named ‘match’ and indexes the matches in each item of the Series.

```python
df1=df['A'].str.extractall(r'(\d+(?:\.\d+)?)')
df1
```



|      | Match |    0    |
| :--: | :---: | :-----: |
|  0   |   0   |    1    |
|  2   |   0   |   10    |
|  3   |   0   | [10,00] |
|  4   |   0   |    0    |
|      |   1   |  4.095  |
|      |   2   |    8    |
|  5   |   0   |   3.2   |
|      |   1   |    8    |
|      |   2   |  9.67   |

The output is a MultiIndex dataframe, with separate rows for each of the numbers identified in the string, Look at row index#4 there are three rows for Match for each of the distinct value found in the Alpha_Numeric String value

