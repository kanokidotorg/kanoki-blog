---
title: "Pandas select rows and columns in MultiIndex dataframe"
date: "2022-07-25"
categories: [ pandas, python]
tags: [ pandas, python]

---

We want to select or slice the rows and columns of a MultiIndex dataframe. In this post we will take a look on how to slice the dataframe using the index at all levels of a row and column 

 A MultiIndex dataframe can have multi index for both rows and columns. The MultiIndex keeps all the defined levels of an index, even if they are not actually used.

There are four methods that could be used to select the rows/columns of MultiIndex dataframe

* Using loc and iloc, which are used to access group of rows and columns by labels and integers respectively
* Dataframe.xs, it returns cross-section from the Series/DataFrame.
* Dataframe.Query, we can query the columns of a DataFrame with a boolean expression.
* IndexSlice, It creates an object to more easily perform multi-index slicing.

We will be following the steps in this order to select rows and columns from a multiindex dataframe 

1. Create a MultiIndex Dataframe
2. Use ***loc*** to slice the dataframe using labels
3. Use ***iloc*** to slice the dataframe based on integer position of Indexes
4. Using Slicers, It slice a MultiIndex by providing multiple indexers
5. Use ***xs*** method, it takes a key argument to select data at a particular level of a MultiIndex
6. ***query*** could be used to select rows based on conditions with help of boolean expression
7. ***IndexSlice*** with default slice command to perform MultiIndex Slicing


## Create MultiIndex Dataframe

We have first created a MultiIndex from the cartesian product of list of rows and columns and after that a dataframe is built using the multiIndex rows and columns.

The dataframe row index has two levels: Region and Division and similarly columns has two levels as well: Quarter(Q1 &Q2) and Buy & Sell.

```python
import pandas as pd
import numpy as np

x = np.round(np.random.uniform(1, 5, size=(9, 4)), 2)

rowIndx = pd.MultiIndex.from_product(
    [["East", "North", "South"], ["A", "B", "C"]],
    names=["Region", "Division"],
)
colIndex = pd.MultiIndex.from_product(
    [["Q1", "Q2"], ["Buy", "Sell"]]
)
multidf = pd.DataFrame(data=x, index=rowIndx, columns=colIndex)
multidf

```


<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
            <td>2.95</td>
            <td>1.06</td>
        </tr> 
        <tr>
	         <td>C</td>    
	         <td>3.77</td>
            <td>4.82</td>
            <td>2.79</td>
            <td>1.07</td>
        </tr>
        <tr>
            <td rowspan="3">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
            <td>2.74</td>
            <td>1.30</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>3.08</td>
            <td>4.04</td>
            <td>3.52</td>
        </tr>    
        <tr>
	         <td>C</td>    
	         <td>1.27</td>
            <td>4.34</td>
            <td>4.18</td>
            <td>4.12</td>
        </tr>
        <tr>
            <td rowspan="3">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.90</td>
            <td>1.45</td>
            <td>2.84</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>4.60</td>
            <td>1.27</td>
            <td>2.81</td>
            <td>3.96</td>
        </tr> 
        <tr>      
	         <td>C</td>    
	         <td>3.25</td>
            <td>1.85</td>
            <td>2.47</td>
            <td>1.46</td>
        </tr> 
    </tbody>
</table>



## Using loc and iloc to slice MultiIndex Dataframe
We want to slice the above dataframe using loc and iloc, let's start with slicing just one single row by Region

### Slice Row at level=0

The row label at level=0 i.e. Region is passed as list to slice the Region East row

```python
multidf.loc[['East']]
```
This will just slice the row with Region East at level=0 and all rows at level=1

 

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
            <td>2.95</td>
            <td>1.06</td>
        </tr>       
	         <td>C</td>    
	         <td>3.77</td>
            <td>4.82</td>
            <td>2.79</td>
            <td>1.07</td>


    </tbody>
</table>   

### Slice Row and Column at level=0

We can slect all the rows starting Region North and just column Q1

```python
multidf.loc['NORTH':, :'Q1':]
```



<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>        
        <tr>
            <td rowspan="3">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>3.08</td>
        </tr>       
	         <td>C</td>    
	         <td>1.27</td>
            <td>4.34</td>
        <tr>
            <td rowspan="3">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.90</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>4.60</td>
            <td>1.27</td>
        </tr>   
        <tr>    
	         <td>C</td>    
	         <td>3.25</td>
            <td>1.85</td>
        </tr>
    </tbody>
</table>


### Slice Column at level=1

We want to slice with column index at level=1, so in this case we would like to see all the rows and just two columns Q1-Sell and Q2-Buy

```python
multidf.loc[:,('Q1', 'Sell'):('Q2', 'Buy')]
```
<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th>Q1</th>
            <th>Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Sell</th>
            <th>Buy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>
            <td>A</td>
            <td>3.88</td>
            <td>4.67</td>
        </tr>  
        <tr>
	         <td>B</td>
            <td>1.05</td>
            <td>2.95</td>
        </tr>       
	         <td>C</td>    
            <td>4.82</td>
            <td>2.79</td>
        <tr>
            <td rowspan="3">North</td>
            <td>A</td>
            <td>1.57</td>
            <td>2.74</td>
        </tr>  
        <tr>
	         <td>B</td>
            <td>3.08</td>
            <td>4.04</td>
        </tr>       
	         <td>C</td>    
            <td>4.34</td>
            <td>4.18</td>
        <tr>
            <td rowspan="3">South</td>
            <td>A</td>
            <td>1.90</td>
            <td>1.45</td>
        </tr>  
        <tr>
	         <td>B</td>
            <td>1.27</td>
            <td>2.81</td>
        </tr>       
	         <td>C</td>    
            <td>1.85</td>
            <td>2.47</td>
    </tbody>
</table>



### Slice row at level=1

Using iloc, we can select the rows and columns based on the integer position at all the levels of rows and columns, Let's see how to do that

We want every second row at level=1 of row index Region and one column at level=1 i.e. Q2-Sell 

```python
multidf.iloc[::2,3:4]
```
This will skip every other row and will select the column at 3rd position at level=1. You can see the row East-B is skipped and thereafter North A & C is skipped.



<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th>Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">East</td>
            <td>A</td>
            <td>1.74</td>
        </tr>  
        <tr>    
	         <td>C</td>    
            <td>1.07</td>
        </tr>
        <tr>
            <td rowspan="1">North</td> 
            <td>B</td>
            <td>3.52</td>           
        </tr>               	         
        <tr>
            <td rowspan="2">South</td>
            <td>A</td>
            <td>2.84</td>
        </tr>  
        <tr>     
	         <td>C</td>    
            <td>1.46</td>
        </tr>
    </tbody>
</table>


## Using slicers to slice a MultiIndex by multiple indexers

We can provide any of the selectors as if we are indexing by label, and we can use slice(None) to select  all the content of that level and don't have to specify all the deeper levels

Let's understand how to use slicer with an example, we want to slice the rows with Region East & North at level=0 and rows B & C at level=1

```python
multidf.loc[(slice('East', 'North'), slice('B', 'C')), :]
```


<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>            
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
            <td>2.95</td>
            <td>1.06</td>
        </tr>       
	         <td>C</td>    
	         <td>3.77</td>
            <td>4.82</td>
            <td>2.79</td>
            <td>1.07</td>
        <tr>
            <td rowspan="3">North</td>        
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>3.08</td>
            <td>4.04</td>
            <td>3.52</td>
        </tr> 
        <tr>      
	         <td>C</td>    
	         <td>1.27</td>
            <td>4.34</td>
            <td>4.18</td>
            <td>4.12</td>
         </tr> 
    </tbody>
</table>


Another Example, here we want to slice the East region and column Q1-Buy

```python
multidf.loc[(slice('East')), (slice('Q1'),slice('Buy'))]
```



<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>   
             <td>A</td>
            <td>3.93</td>   
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
        </tr> 
        <tr>      
	         <td>C</td>    
	         <td>3.77</td>
        </tr>        
    </tbody>
</table>


If you want both Q1 & Q2 buy columns

```python
multidf.loc[(slice('East')), (slice('Q1','Q2'),slice('Buy'))]
```



<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="1">Q1</th>
            <th colspan="1">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Buy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>  
              <td>A</td>
            <td>3.93</td>
            <td>4.67</td>    
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>2.95</td>
        </tr>       
	         <td>C</td>    
	         <td>3.77</td>
            <td>2.79</td>
        <tr>
            <td rowspan="3">North</td>    
            <td>A</td>
            <td>4.89</td>
            <td>2.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>4.04</td>
        </tr> 
        <tr>      
	         <td>C</td>    
	         <td>1.27</td>
            <td>4.18</td>
         </tr> 
    </tbody>
</table>


## Using xs to slice a MultiIndex dataframe

This method(xs) returns a cross-section from the Series/DataFrame. It takes a key argument to select data at a particular level of a MultiIndex.

we can also specify the level to indicate which levels are used. Levels can be referred by label or position

Select rows for Region East, we can also specify the level=0 in this case

```python
multidf.xs('East', level=0)
```



<table>
    <thead>
        <tr>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
            <td>2.95</td>
            <td>1.06</td>
        </tr>       
	         <td>C</td>    
	         <td>3.77</td>
            <td>4.82</td>
            <td>2.79</td>
            <td>1.07</td>
    </tbody>
</table>

we can also slice by multiple levels by passing levels as list, here we want to select the rows with Region North at level=0 and Division C at level=1

```python
multidf.xs(('North', 'C'), level=[0,1])
```

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th>Q1</th>
            <th>Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Sell</th>
            <th>Buy</th>
        </tr>
    </thead>
    <tbody>        
        <tr>
             <td rowspan="3">North</td>
	         <td>C</td>    
            <td>4.34</td>
            <td>4.18</td>
        </tr>   
   </tbody>
</table>

We can also slice columns, Here we want only Q1-Buy so the column index level is passed as [0, 1] and axis=1



```python
multidf.xs(('Q1', 'Buy'), level=[0,1], axis=1)
```

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>
            <td>A</td>
            <td>3.93</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
        </tr>       
	         <td>C</td>    
	         <td>3.77</td>
        <tr>
            <td rowspan="3">North</td>
            <td>A</td>
            <td>4.89</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
        </tr>       
	         <td>C</td>    
	         <td>1.27</td>
        <tr>
            <td rowspan="3">South</td>
            <td>A</td>
            <td>4.07</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>4.60</td>
        </tr> 
        <tr>      
	         <td>C</td>    
	         <td>3.25</td>
        </tr> 
    </tbody>
</table>


## Using query to slice a MultiIndex dataframe based on condition

We want to query the columns of a DataFrame with a boolean expression. It takes an expression to be evaluated as parameter and returns the DataFrame resulting from the provided query expression.

We want to get all the rows where division is 'A' 

```python
multidf.query("Division == 'A'")
```
<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="1">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>         
        <tr>
            <td rowspan="1">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
            <td>2.74</td>
            <td>1.30</td>
        </tr>        
        <tr>
            <td rowspan="1">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.90</td>
            <td>1.45</td>
            <td>2.84</td>
        </tr>  
     </tbody>
</table>


Select all the rows in Division A & B

```python
multidf.query("Division.isin(['A', 'B'])")
```
<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
            <td>2.95</td>
            <td>1.06</td>
        </tr>       	       
        <tr>
            <td rowspan="2">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
            <td>2.74</td>
            <td>1.30</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>3.08</td>
            <td>4.04</td>
            <td>3.52</td>
        </tr>       	  
        <tr>
            <td rowspan="2">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.90</td>
            <td>1.45</td>
            <td>2.84</td>
        </tr>         
    </tbody>
</table>


## Using get\_level_values()

This method returns an Index of values for requested level and useful to get an individual level of values from a MultiIndex

Select all rows where division is A, you can pass the label as parameter to get_level_values() method

```python
multidf[multidf.index.get_level_values('Division') == 'A']
```

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="1">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>         
        <tr>
            <td rowspan="1">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
            <td>2.74</td>
            <td>1.30</td>
        </tr>        
        <tr>
            <td rowspan="1">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.90</td>
            <td>1.45</td>
            <td>2.84</td>
        </tr>  
     </tbody>
</table>


We can also filter the column index, here we want all the Buy columns in the dataframe, you can also pass the index integer position as parameter

```python
multidf.loc[:, multidf.columns.get_level_values(1) == 'Buy']
```
<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="1">Q1</th>
            <th colspan="1">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Buy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>4.67</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>2.95</td>
        </tr>       
	         <td>C</td>    
	         <td>3.77</td>
            <td>2.79</td>
        <tr>
            <td rowspan="3">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>2.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>4.04</td>
        </tr>       
	         <td>C</td>    
	         <td>1.27</td>
            <td>4.18</td>
        <tr>
            <td rowspan="3">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.45</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>4.60</td>
            <td>2.81</td>
        </tr> 
        <tr>      
	         <td>C</td>    
	         <td>3.25</td>
            <td>2.47</td>
        </tr> 
    </tbody>
</table>


## Select rows based on conditions

Let's select the rows based on conditions, we will use get_level_values() and loc methods to filter the dataframe

We will first define our MultiIndex condition and save it in a variable, here we want Q1-Sell>2.5 and Q1-Buy>4 and Region is North and South

```python
condition = ((multidf[( 'Q1',  'Sell')]>2.5)
           &(multidf[( 'Q1',  'Buy')]>4)
           &(multidf.index.get_level_values(0).isin(['South', 'North'])))
multidf[condition]
```

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
       <tr>
            <td rowspan="1">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
            <td>2.74</td>
            <td>1.30</td>
        </tr>          
        <tr>
            <td rowspan="1">South</td>
            <td>C</td>
            <td>3.25</td>
            <td>1.85</td>
            <td>2.47</td>
            <td>1.46</td>
        </tr>          
    </tbody>
</table>


Alternatively we can also use [np.where()](https://kanoki.org/2020/01/03/how-to-work-with-numpy-where/) to filter the rows based on condition. 

The numpy.where() function just returns the index of all the matching rows which are evaluated true for the condition. 

```python
multidf.iloc[np.where(condition)]
```

Another example where we want row where Q-Sell is maximum. we are using argmax that returns the index of the maximum value along an axis

```python
multidf.iloc[np.argmax(multidf[( 'Q1',  'Sell')])]
```

**Out:**

```python
Q1   Buy     1.08
     Sell    4.54
Q2   Buy     4.73
     Sell    2.19
Name: (North, C), dtype: float64
```

## Using IndexSlice

It creates an object to perform multi-index slicing, which means we don't have to construct the slices on our own. 

All usages of colons : are converted into slice object. If multiple arguments are passed to the index operator, the arguments are turned into n-tuples

```python
idx = pd.IndexSlice
multidf.loc[idx[:, 'A':'B'], :]
```
It returns all the Division rows A and B for all Regions

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
            <td>2.95</td>
            <td>1.06</td>
        </tr>       
        <tr>
            <td rowspan="2">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
            <td>2.74</td>
            <td>1.30</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>3.08</td>
            <td>4.04</td>
            <td>3.52</td>
        </tr>       
        <tr>
            <td rowspan="2">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.90</td>
            <td>1.45</td>
            <td>2.84</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>4.60</td>
            <td>1.27</td>
            <td>2.81</td>
            <td>3.96</td>
        </tr> 
    </tbody>
</table>


So IndexSlice requires you to specify enough levels of the MultiIndex to remove an ambiguity

Here we are slicing by Region and Division both

```python
idx = pd.IndexSlice
multidf.loc[idx['East':'North', 'A':'B'], :]
```
<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
            <th colspan="2">Q2</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
            <td>4.67</td>
            <td>1.74</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
            <td>2.95</td>
            <td>1.06</td>
        </tr>       
        <tr>
            <td rowspan="2">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
            <td>2.74</td>
            <td>1.30</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>3.08</td>
            <td>4.04</td>
            <td>3.52</td>
        </tr>               
    </tbody>
</table>


we can also slice the column, here we want only Q1 at level=0 for all the Regions and Divisions

```python
idx = pd.IndexSlice
multidf.loc[:, :'Q1']

```

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th colspan="2">Q1</th>
        </tr>
        <tr>
            <th>Region</th>
            <th>Division</th>
            <th>Buy</th>
            <th>Sell</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="3">East</td>
            <td>A</td>
            <td>3.93</td>
            <td>3.88</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.88</td>
            <td>1.05</td>
        </tr>       
	         <td>C</td>    
	         <td>3.77</td>
            <td>4.82</td>
        <tr>
            <td rowspan="3">North</td>
            <td>A</td>
            <td>4.89</td>
            <td>1.57</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>3.57</td>
            <td>3.08</td>
        </tr>       
	         <td>C</td>    
	         <td>1.27</td>
            <td>4.34</td>
        <tr>
            <td rowspan="3">South</td>
            <td>A</td>
            <td>4.07</td>
            <td>1.90</td>
        </tr>  
        <tr>
	         <td>B</td>
	         <td>4.60</td>
            <td>1.27</td>
        </tr> 
        <tr>      
	         <td>C</td>    
	         <td>3.25</td>
            <td>1.85</td>
        </tr> 
    </tbody>
</table>
