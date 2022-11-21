---
title: "Pandas rolling apply custom functions and statistical operations such as rolling mean(), max(), count(), sum(), median() agg()"	
date: 2022-11-21
categories: [python, pandas]
tags: [python, pandas]
last_modified_at: 2022-11-21
permalink: Pandas-rolling-apply-custom-func-and-rolling-mean

---

Rolling window calculations are provided by Pandas rolling() function. 

The rolling() function is commonly used in finance, economics, and science. It is utilised to work with time series data. 

Rolling is the process of establishing a window that rolls through the data with a predetermined size to do calculations. Window calculation format is [n] and [n-1].

```python
DataFrame.rolling(window, min_periods=None, center=False, win_type=None, on=None, axis=0, closed=None, step=None, method='single')
```
### **Data**

We will be working with yahoo finance data. It contains information about Google's stock data from April 1, 2020 to March 31, 2022 - data for two fiscal years. 

The data only includes trading days, or days when the stock market was open for business.

```python
# importing required libraries
import pandas as pd
import numpy as np
import yfinance as yf

# setting the start and end date for our data
start_date = '2020-04-01'
end_date = '2022-03-31'

# getting GOOGLE stocks data from yfinance
data = yf.Ticker('GOOGL').history(start = start_date, end = end_date, actions =  False)

# printing our data
data
```

This is how our data looks like

|                           |       Open |       High |        Low |      Close |   Volume |
|--------------------------:|-----------:|-----------:|-----------:|-----------:|---------:|
|                      Date |            |            |            |            |          |
| 2020-04-01 00:00:00-04:00 |  56.200001 |  56.471001 |  54.674500 |  55.105000 | 51970000 |
| 2020-04-02 00:00:00-04:00 |  55.000000 |  56.138500 |  54.656502 |  55.851501 | 56410000 |
| 2020-04-03 00:00:00-04:00 |  55.735500 |  55.939499 |  53.754002 |  54.634998 | 51374000 |
| 2020-04-06 00:00:00-04:00 |  56.650002 |  59.537498 |  56.250000 |  59.159500 | 63320000 |
| 2020-04-07 00:00:00-04:00 |  60.850498 |  61.039001 |  58.862499 |  59.127998 | 61620000 |
|                       ... |        ... |        ... |        ... |        ... |      ... |
| 2022-03-24 00:00:00-04:00 | 139.199997 | 141.619003 | 137.750504 | 141.572006 | 26396000 |
| 2022-03-25 00:00:00-04:00 | 141.916000 | 142.035004 | 139.737503 | 141.673004 | 24126000 |
| 2022-03-28 00:00:00-04:00 | 140.900497 | 142.002502 | 139.811493 | 141.455505 | 35050000 |
| 2022-03-29 00:00:00-04:00 | 142.647507 | 143.793503 | 142.038498 | 142.505493 | 34318000 |
| 2022-03-30 00:00:00-04:00 | 142.460007 | 142.720505 | 141.600006 | 141.938507 | 19884000 |

504 rows × 5 columns

## Pandas Rolling function parameters

### *window*
Window calculation format is [n] and [n-1]. The rolling size of the data is the window size. Different approaches exist for applying window size to the data. 

Use of integer value is one approach, it counts the number of integers and performs the function. Using period markers is an another method for calculating by time periods.<br>

We will be applying rolling to the *'Close'* column of our data.

```python
data['Close_Sum'] = data['Close'].rolling(window = 7).sum()

data.head(10)
```
We are using addition operation for this part. The output shows 6 NaN values because it does not have enough values to add. The window calculation starts at the 7th column as 7 is our window size.

In this section, we will use the addition operation - .sum(). Because there aren't enough values to add, the output contains 6 NaN values. The window calculation begins in the seventh column, as 7 is the size of our window.

|                           |     Close |  Close_Sum |
|--------------------------:|----------:|-----------:|
|                      Date |           |            |
| 2020-04-01 00:00:00-04:00 | 55.105000 |        NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |        NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |        NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |        NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |        NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |        NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 404.557495 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 409.972996 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 417.382996 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 425.612999 |

**Note**: When window = 1 the data will show no NaN values as 1 is the minimum window size for calculation. This means [n] always exists as the window is set to 1.

### *min_periods*
min_periods default value is equal to window size. It assigns minimum size of computations to be performed.

```python
data['Close_Sum'] = data['Close'].rolling(window = 7, min_periods = 5).sum()

data.head(10)
```
The output shows the calculation starts at the 5th column as the min_periods is set to 5.

|                           |     Close |  Close_Sum |
|--------------------------:|----------:|-----------:|
|                      Date |           |            |
| 2020-04-01 00:00:00-04:00 | 55.105000 |        NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |        NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |        NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |        NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 | 283.878998 |
| 2020-04-08 00:00:00-04:00 | 60.349998 | 344.228996 |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 404.557495 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 409.972996 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 417.382996 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 425.612999 |

When min_periods is greater than window size it will show *ValueError* as min_periods should always be equal to or less than window size.<br> min_periods <= window

```python
data['Close_Sum'] = data['Close'].rolling(window = 7, min_periods = 8).sum()

data.head(10)
```
Error
```python
ValueError: min_periods 8 must be <= window 7
```

### *center*
center default is set to False. It is used to begin the window calculation at the center of the window's size.

```python
data['Close_Sum'] = data['Close'].rolling(window = 10, center = True).sum()

data.head(10)
```

The calculation begins at the fifth column because 5 is the centre of 10.

|                           |     Close |  Close_Sum |
|--------------------------:|----------:|-----------:|
|                      Date |           |            |
| 2020-04-01 00:00:00-04:00 | 55.105000 |        NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |        NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |        NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |        NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |        NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 | 591.204498 |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 598.970997 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 607.069496 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 615.491997 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 616.940498 |

### *win_type*
win_type generates a generic rolling window calculation that is weighted based weights. Weights are normally normalised to the largest weight and uniformly weighted by default. <br>

The list of recognized types are as follows:<br>
*triang, boxcar, blackman, hamming, bartlett, parzen, bohman, blackmanharris, nuttall, barthann, kaiser (beta), gaussian (std), general_gaussian (power, width), slepian (width)*

We will use some of these types in this article.

### *triang*
*triang* does the triangular calculation of the window size.

```python
data['Triang_Mean'] = data['Close'].rolling(window = 6, win_type = 'triang').mean()

data.head(10)
```

|                           |     Close | Triang_Mean |
|--------------------------:|----------:|------------:|
|                      Date |           |             |
| 2020-04-01 00:00:00-04:00 | 55.105000 |         NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |         NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |         NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |         NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |         NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |   57.186999 |
| 2020-04-09 00:00:00-04:00 | 60.328499 |   58.476249 |
| 2020-04-13 00:00:00-04:00 | 60.520500 |   59.500527 |
| 2020-04-14 00:00:00-04:00 | 63.261501 |   60.264388 |
| 2020-04-15 00:00:00-04:00 | 62.865002 |   60.948472 |

### *boxcar*

```python
data['Boxcar_Mean'] = data['Close'].rolling(window = 5, win_type = 'boxcar').mean()

data.head(10)
```

|                           |     Close | Boxcar_Mean |
|--------------------------:|----------:|------------:|
|                      Date |           |             |
| 2020-04-01 00:00:00-04:00 | 55.105000 |         NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |         NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |         NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |         NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |   56.775800 |
| 2020-04-08 00:00:00-04:00 | 60.349998 |   57.824799 |
| 2020-04-09 00:00:00-04:00 | 60.328499 |   58.720199 |
| 2020-04-13 00:00:00-04:00 | 60.520500 |   59.897299 |
| 2020-04-14 00:00:00-04:00 | 63.261501 |   60.717699 |
| 2020-04-15 00:00:00-04:00 | 62.865002 |   61.465100 |

**Note**: boxcar window is equivalent to mean()

### *gaussian with standard deviation*
Gaussian mean is calculated using std. std is short for Standard Deviation.

```python
data['Gaussian_Mean'] = data['Close'].rolling(window = 5, win_type = 'gaussian').mean(std = 0.25)

data.head(10)
```

|                           |     Close | Gaussian_Mean |
|--------------------------:|----------:|--------------:|
|                      Date |           |               |
| 2020-04-01 00:00:00-04:00 | 55.105000 |           NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |           NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |           NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |           NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |     54.636923 |
| 2020-04-08 00:00:00-04:00 | 60.349998 |     59.157973 |
| 2020-04-09 00:00:00-04:00 | 60.328499 |     59.128419 |
| 2020-04-13 00:00:00-04:00 | 60.520500 |     60.349582 |
| 2020-04-14 00:00:00-04:00 | 63.261501 |     60.328570 |
| 2020-04-15 00:00:00-04:00 | 62.865002 |     60.521355 |

### *on*
This is an optional parameter of .rolling() function. *on* is to specify a column rather than the default of the index in a dataframe.

```python
data.rolling(window = 10, on = 'Close').sum()
```

### *axis*
0 or index; default, 1 or column<br> It is set at the axis of the data.

```python
data.rolling(window = 10, axis = 0).sum()
```

### *closed*
If 'right', the first point in the window is excluded from calculations.<br>
If 'left', the last point in the window is excluded from calculations.<br>
If 'both', the no points in the window are excluded from calculations.<br>
If 'neither', the first and last points in the window are excluded from calculations.<br>
*default* None ('right').

```python
data.rolling(window = 10, closed = left).sum()
```

### *step*
int, default None
Evaluate the window at every step result, equivalent to slicing as [::step]. window must be an integer. Using a step argument other than None or 1 will produce a result with a different shape than the input.

```python
data.rolling(window = 10, step = 2).sum()
```

### *method*
str {'single', 'table'}, default 'single'
Execute the rolling operation per single column or row ('single') or over the entire object ('table'). This argument is only implemented when specifying engine='numba' in the method call.

```python
data.rolling(window = 10, method = 'single').sum()
```

## Pandas rolling Apply functions

We want to get the difference between the first and second row in each rolling window.

The apply() function with lambda function uses iloc to get the first and second row and computes the difference between them

```python
data['Difference_lambda'] = data['Close'].rolling(7).apply(lambda x: x.iloc[1] - x.iloc[0])

data.head(10)
```

Output:

|                           |     Close | Difference |
| ------------------------: | --------: | ---------: |
|                      Date |           |            |
| 2020-04-01 00:00:00-04:00 | 55.105000 |        NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |        NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |        NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |        NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |        NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |        NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 |   0.746502 |
| 2020-04-13 00:00:00-04:00 | 60.520500 |  -1.216503 |
| 2020-04-14 00:00:00-04:00 | 63.261501 |   4.524502 |
| 2020-04-15 00:00:00-04:00 | 62.865002 |  -0.031502 |

## Pandas rolling Apply functions on multiple columns

We want to apply a custom function on all the columns of our data, In this case we will find the difference between the max and min value for each rolling window of data and then compute the square of the difference

Let's create a custom function to do this

```python
def max_min(win):
    return (win.max()-win.min())**2
```

Let's pass the above function(max_min()) to apply each column of the rolling window

```python
data.rolling(7).apply(max_min)
```

Output:



|                           | Open      | High      | Low       |     Close |       Volume |
| ------------------------: | --------- | --------- | --------- | --------: | -----------: |
|                      Date |           |           |           |           |              |
| 2020-04-01 00:00:00-04:00 | NaN       | NaN       | NaN       |       NaN |          NaN |
| 2020-04-02 00:00:00-04:00 | NaN       | NaN       | NaN       |       NaN |          NaN |
| 2020-04-03 00:00:00-04:00 | NaN       | NaN       | NaN       |       NaN |          NaN |
| 2020-04-06 00:00:00-04:00 | NaN       | NaN       | NaN       |       NaN |          NaN |
| 2020-04-07 00:00:00-04:00 | NaN       | NaN       | NaN       |       NaN |          NaN |
| 2020-04-08 00:00:00-04:00 | NaN       | NaN       | NaN       |       NaN |          NaN |
| 2020-04-09 00:00:00-04:00 | 34.916286 | 26.625598 | 34.421651 | 32.661227 | 5.283562e+14 |
| 2020-04-13 00:00:00-04:00 | 34.916286 | 26.625598 | 34.421651 | 34.639132 | 6.060459e+14 |
| 2020-04-14 00:00:00-04:00 | 39.225175 | 61.591097 | 58.874876 | 74.416554 | 6.079183e+14 |
| 2020-04-15 00:00:00-04:00 | 32.211288 | 18.062500 | 29.702508 | 17.085847 | 6.079183e+14 |

## Pandas rolling mathematical & statistical Operations

### Computations with .rolling()

we will be using with .rolling() wuth pandas mathematical and statistical operations. We will also see cumulative operations used for windows calculation. We will now learn the following calculations with .rolling() function.

- Mathematical & Statistical Operations:<br>
  *count(), sum(), mean(), median(), min(), max(), agg(), std(), var(), skew(), kurt(), quantile(), apply(), cov(), corr()*
- Cumulative Operations:<br>
  *cumsum() , cumprod() , cummin(), cummax()*

### rolling().count()
.count() measures the rolling count of any non-NaN observations inside the window.

```python
data['Count'] = data['Close'].rolling(7).count()

data.head(10)
```
The output

|                           |     Close | Count |
|--------------------------:|----------:|------:|
|                      Date |           |       |
| 2020-04-01 00:00:00-04:00 | 55.105000 |   1.0 |
| 2020-04-02 00:00:00-04:00 | 55.851501 |   2.0 |
| 2020-04-03 00:00:00-04:00 | 54.634998 |   3.0 |
| 2020-04-06 00:00:00-04:00 | 59.159500 |   4.0 |
| 2020-04-07 00:00:00-04:00 | 59.127998 |   5.0 |
| 2020-04-08 00:00:00-04:00 | 60.349998 |   6.0 |
| 2020-04-09 00:00:00-04:00 | 60.328499 |   7.0 |
| 2020-04-13 00:00:00-04:00 | 60.520500 |   7.0 |
| 2020-04-14 00:00:00-04:00 | 63.261501 |   7.0 |
| 2020-04-15 00:00:00-04:00 | 62.865002 |   7.0 |

### rolling().sum()
.sum() calculates the rolling sum of given data.

```python
data['Sum'] = data['Close'].rolling(7).sum()

data.head(10)
```
We will be using 7 as our window size. The output is as follows:

|                           |     Close |        Sum |
|--------------------------:|----------:|-----------:|
|                      Date |           |            |
| 2020-04-01 00:00:00-04:00 | 55.105000 |        NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |        NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |        NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |        NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |        NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |        NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 404.557495 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 409.972996 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 417.382996 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 425.612999 |

### rolling().mean() - moving average

In finance, moving average is a stock indicator. The moving average helps to level the price data over a specified period by creating a constantly updated average price.

```python
data['Mean'] = data['Close'].rolling(7).mean()

data.head(10)
```
The output of mean with window size of 7.

|                           |     Close |      Mean |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 57.793928 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 58.567571 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 59.626142 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 60.801857 |

### rolling().median()
median() calculates the arithmetic median of values.

```python
data['Median'] = data['Close'].rolling(7).median()

data.head(10)
```
The output of median with window size of 7.

|                           |     Close |    Median |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 59.127998 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 59.159500 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 60.328499 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 60.349998 |

### rolling().min()
min() calculates the minimum value of the column.

```python
data['Min'] = data['Close'].rolling(7).min()

data.head(10)
```
The output

|                           |     Close |       Min |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 54.634998 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 54.634998 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 54.634998 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 59.127998 |

### rolling().max()
max() calculates the maximum value of the column.

```python
data['Max'] = data['Close'].rolling(7).max()

data.head(10)
```
The output

|                           |     Close |       Max |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 60.349998 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 60.520500 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 63.261501 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 63.261501 |

### rolling().agg()

.agg() function is to calculate aggregate of the column.

```python
data['Aggregate'] = data['Close'].rolling(7).agg(['mean'])

data.head(10)
```
The output

|                           |     Close | Aggregate |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 57.793928 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 58.567571 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 59.626142 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 60.801857 |

The same can be done by replacing mean with sum or std.

### rolling().std()
.std calculates the rolling standard deviation.

```python
data['Standard_Deviation'] = data['Close'].rolling(7).std()

data.head(10)
```
The output for standard deviation calculation.

|                           |     Close | Aggregate |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 57.793928 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 58.567571 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 59.626142 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 60.801857 |

Gaussian function is calculated with .std().<br>
We can also use value with std()
```python
data['Standard_Deviation'] = data['Close'].rolling(7).std(0.25)
```
### rolling().var()
Unbiased variance

```python
data['Variance'] = data['Close'].rolling(7).var()

data.head(10)
```
The output for variance.

|                           |     Close | Variance |
|--------------------------:|----------:|---------:|
|                      Date |           |          |
| 2020-04-01 00:00:00-04:00 | 55.105000 |      NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |      NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |      NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |      NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |      NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |      NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 6.264045 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 5.599745 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 6.735067 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 2.718827 |

### rolling().skew()
.skew() calculates the skewness of the data. *axis* is passed as argument to calculate on the particular axis. Minimum window size of 3 is required to perform skewness.
```python
data['Skewness'] = data['Close'].rolling(7).skew()

data.head(10)
```
The output for skew function.

|                           |     Close |  Skewness |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | -0.303467 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | -1.089958 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | -1.002235 |
| 2020-04-15 00:00:00-04:00 | 62.865002 |  0.745431 |

### rolling().kurt()
.kurt() calculates unbiased rolling kurtosis. Minimum window size of 4 is required to perform kurtosis.

```python
data['Kurtosis'] = data['Close'].rolling(7).kurt()

data.head(10)
```
The output for kutosis function.

|                           |     Close |  Kurtosis |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | -2.347140 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | -0.517601 |
| 2020-04-14 00:00:00-04:00 | 63.261501 |  2.794329 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | -0.984423 |

### rolling().quantile()

.quantile() is for calculate the rolling quantile of the data. A float value is required to be passed for the calculation.

```python
data['Quantile'] = data['Close'].rolling(7).quantile(0.25)

data.head(10)
```
The output for quantile calculation.

|                           |     Close |  Quantile |
|--------------------------:|----------:|----------:|
|                      Date |           |           |
| 2020-04-01 00:00:00-04:00 | 55.105000 |       NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |       NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |       NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |       NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |       NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |       NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 | 55.478251 |
| 2020-04-13 00:00:00-04:00 | 60.520500 | 57.489750 |
| 2020-04-14 00:00:00-04:00 | 63.261501 | 59.143749 |
| 2020-04-15 00:00:00-04:00 | 62.865002 | 59.743999 |

### rolling().cov()
.cov() calculates the covariance of the column. There are two ways to use .cov(). We will see how to find covariance of one column and by another column.

- Calculating covariance by one column.
```python
data['Covariance'] = data['Close'].rolling(7).cov()

data.head(10)
```
The output for covariance of Close.

|                           |     Close | Covariance |
|--------------------------:|----------:|-----------:|
|                      Date |           |            |
| 2020-04-01 00:00:00-04:00 | 55.105000 |        NaN |
| 2020-04-02 00:00:00-04:00 | 55.851501 |        NaN |
| 2020-04-03 00:00:00-04:00 | 54.634998 |        NaN |
| 2020-04-06 00:00:00-04:00 | 59.159500 |        NaN |
| 2020-04-07 00:00:00-04:00 | 59.127998 |        NaN |
| 2020-04-08 00:00:00-04:00 | 60.349998 |        NaN |
| 2020-04-09 00:00:00-04:00 | 60.328499 |   6.264045 |
| 2020-04-13 00:00:00-04:00 | 60.520500 |   5.599745 |
| 2020-04-14 00:00:00-04:00 | 63.261501 |   6.735067 |
| 2020-04-15 00:00:00-04:00 | 62.865002 |   2.718827 |

- Calculating covariance by passing a column

```python
data['Covariance'] = data['Open'].rolling(7).cov(data['Close'])
```
Here, we measured covariance between Open and Close.

### rolling().corr()
.corr() calculates the correlation of the data. It is used to find correlations between two time series data.

```python
data['Correlation'] = data['Open'].rolling(7).corr(data['Close'])

data.head(10)
```
The output gives the correlation value between Open and Close column.

|                           |      Open |     Close | Correlation |
|--------------------------:|----------:|----------:|------------:|
|                      Date |           |           |             |
| 2020-04-01 00:00:00-04:00 | 56.200001 | 55.105000 |         NaN |
| 2020-04-02 00:00:00-04:00 | 55.000000 | 55.851501 |         NaN |
| 2020-04-03 00:00:00-04:00 | 55.735500 | 54.634998 |         NaN |
| 2020-04-06 00:00:00-04:00 | 56.650002 | 59.159500 |         NaN |
| 2020-04-07 00:00:00-04:00 | 60.850498 | 59.127998 |         NaN |
| 2020-04-08 00:00:00-04:00 | 60.154999 | 60.349998 |         NaN |
| 2020-04-09 00:00:00-04:00 | 60.909000 | 60.328499 |    0.838328 |
| 2020-04-13 00:00:00-04:00 | 60.075001 | 60.520500 |    0.843483 |
| 2020-04-14 00:00:00-04:00 | 61.998501 | 63.261501 |    0.834111 |
| 2020-04-15 00:00:00-04:00 | 62.325500 | 62.865002 |    0.717847 |

## Cumulative Operations
Below are the cumulative operations which can be performed in windows calculation. These operations do not use .rolling() function.

### cumsum()

```python
data['Cumulative_Sum'] = data['Close'].cumsum()
```
### cumprod()
```python
data['Cumulative_Product'] = data['Close'].cumprod()
```
### cummin()
```python
data['Cumulative_Minimum'] = data['Close'].cummin()
```
### cummax()
```python
data['Cumulative_Maximum'] = data['Close'].cummax()
```

## Time-aware rolling
### *s, min, h, w, m, y*

Passing an offset (or convertible) to .rolling() method will produce variable sized windows based on the passed window time. For each time point, it will include all preceding values occurring within the indicated time delta. This is particularly useful for a non-regular time frequency index.

### seconds - s
```python
data['Seconds'] = data['Close'].rolling(window = '30s').mean()
```

### minute - min
```python
data['Minute'] = data['Close'].rolling(window = '30min').mean()
```

### hour - h
```python
data['Hour'] = data['Close'].rolling(window = '3h').mean()
```

### business days
```python
data.rolling(window = 30).mean()
```

### calender days - d
```python
data.rolling(window = '30D').mean()
```

### months - m
```python
data.rolling(window = '3M').mean()
```

### year - y
```python
data.rolling(window = '1Y').mean()
```