---
title: "Pandas in a nutshell"
date: "2017-07-16"
categories: [ Data Science, Python ]
tags: [ Pandas, Python ]
---

# Introduction

Pandas is an open source data analysis library in Python and it is extensively used for Data analysis, Data munging and Cleaning. Pandas has a high performing and user friendly data structures.

What makes Pandas a great choice for data analysis? It is it’s rich and highly performant data structures which are built on top of numpy and leverage it’s fast array operations. In thispost we will see how Pandas can be used efficiently for the data analysis and will explore some of it’s basic operations.
Pandas has two types of data structures:

a) Series – it’s a one dimensional array with indexes, it stores a single column or row of data in a Dataframe

b) Dataframe – it’s a tabular spreadsheet like structure representing rows each of which contains one or multiple columns

First, Let’s do some import

```
<span class="kn" style="color: #008080;">import</span> <span class="nn" style="color: #0000ff;">pandas</span> <span class="k" style="color: #008080;">as</span> <span class="nn" style="color: #0000ff;">pd</span>
<span class="kn" style="color: #008080;">import</span> <span class="nn" style="color: #0000ff;">numpy</span> <span class="k" style="color: #008080;">as</span> <span class="nn" style="color: #0000ff;">np</span>
<span class="kn" style="color: #008080;">from</span> <span class="nn" style="color: #0000ff;">pandas</span> <span class="k" style="color: #008080;">import</span> <span class="n" style="color: #0000ff;">Series</span><span class="p">,</span> <span class="n" style="color: #0000ff;">DataFrame
</span>
<span class="kn" style="color: #008080;">import</span> <span class="nn" style="color: #0000ff;">pandas</span> <span class="k" style="color: #008080;">as</span> <span class="nn" style="color: #0000ff;">pd</span>
<span class="kn" style="color: #008080;">import</span> <span class="nn" style="color: #0000ff;">numpy</span> <span class="k" style="color: #008080;">as</span> <span class="nn" style="color: #0000ff;">np</span>
<span class="kn" style="color: #008080;">from</span> <span class="nn" style="color: #0000ff;">pandas</span> <span class="k" style="color: #008080;">import</span> <span class="n" style="color: #0000ff;">Series</span><span class="p">,</span> <span class="n" style="color: #0000ff;">DataFrame
</span>
```

## Series

Create a Series with the list of values and default indexes

```
<span class="n">this</span><span class="o">=</span><span class="n">Series</span><span class="p">([</span><span style="color: #993300;"><span class="mi">6</span><span class="p">,</span><span class="mi">5</span><span class="p">,</span><span class="mi">4</span><span class="p">,</span><span class="mi">9</span><span class="p">,</span><span class="mi">2</span></span><span class="p">])</span>
<span class="n">this</span><span class="o">=</span><span class="n">Series</span><span class="p">([</span><span style="color: #993300;"><span class="mi">6</span><span class="p">,</span><span class="mi">5</span><span class="p">,</span><span class="mi">4</span><span class="p">,</span><span class="mi">9</span><span class="p">,</span><span class="mi">2</span></span><span class="p">])</span>
```

Check how the Series object looks like

```
<span class="n">this</span>
```

```
0 6
1 5
2 4
3 9
4 2
dtype: int6
```

```
<span class="n">this</span><span class="o">.</span><span class="n">values</span>
```

```
array([6, 5, 4, 9, 2])
```

Check the Index for this Series, Pandas generates the indexes automatically and default starts from 0

```
<span class="n">this</span><span class="o">.</span><span class="n">index</span>
```

```
Int64Index([0, 1, 2, 3, 4], dtype=’int64′)
```

Create a Series of US Cities with their Population and Index is set as the City for the Series, The Object takes the Indexes as defined with their corresponding column values i.e Population

```
citypopulation_2016 = Series([8537673,3976322,2704958,2303482,1615017],
index=[‘NYC’,‘Los Angeles’,‘Chicago’,‘Houston’,‘Phoenix’])
```

```
<span class="n">citypopulation_2016</span>
```

```
NYC 8537673
Los Angeles 3976322
Chicago 2704958
Houston 2303482
Phoenix 1615017
dtype: int64
```

## Find Population lesser than 2.5 million

Find the rows with population lower than 2.5 million

```
<span class="n">citypopulation_2016</span><span class="p">[</span><span style="color: #993300;"><span class="n">citypopulation_2016</span><span class="o">&lt;</span><span class="mi">2500000</span></span><span class="p">]</span>
```

```
Houston 2303482
Phoenix 1615017
dtype: int64
```

## Is Chicago in the Series

Check if Chicago is in the Series or not, it will return True or False boolean value

```
<span class="s">'Chicago'</span> <span class="ow">in</span> <span class="n">citypopulation_2016</span>
```

```
True
```

## Create a list of Index

Create a list of Indexes with one extra value Philadelphia

```
<span class="n">cities</span> <span class="o">=</span> <span class="p">[</span><span style="color: #993300;"><span class="s">'NYC'</span><span class="p">,</span><span class="s">'Los Angeles'</span><span class="p">,</span><span class="s">'Chicago'</span><span class="p">,</span><span class="s">'Houston'</span><span class="p">,</span><span class="s">'Phoenix'</span><span class="p">,</span><span class="s">'Philadelhia'</span></span><span class="p">]</span>
```

Create a Series with this new Indexes, The value for Philadelhia is NaN (Not a Number)

```
<span class="n">Ser</span> <span class="o">=</span> <span class="n">Series</span><span class="p">(</span><span class="n">citypopulation_2016</span><span class="p">,</span><span class="n">index</span><span class="o">=</span><span class="n">cities</span><span class="p">)</span>
```

```
<span class="n">Ser</span>
```

```
NYC 8537673
Los Angeles 3976322
Chicago 2704958
Houston 2303482
Phoenix 1615017
Philadelhia NaN
dtype: float64
```

Philadelphia not in list that’s why it is null

Check what are the null values in the above Series
```
<span class="n">Ser</span><span class="o">.</span><span class="n">isnull</span><span class="p">()</span>
```

```
NYC False
Los Angeles False
Chicago False
Houston False
Phoenix False
Philadelhia True
dtype: bool
```

Adding two series

Add the two Series object Ser & citypopulation_2016 and store the sum to a new object Ser1

```
<span class="n">Ser1</span> <span class="o">=</span> <span class="n">Ser</span><span class="o">+</span><span class="n">citypopulation_2016</span>
<span class="n">Ser1</span>
```

```
Chicago 5409916
Houston 4606964
Los Angeles 7952644
NYC 17075346
Philadelhia NaN
Phoenix 3230034
dtype: float64
```

Name the Series

```
<span class="n">Ser1</span><span class="o">.</span><span class="n">name</span> <span class="o">=</span> <span class="s">"US Populate cities"</span>
<span class="n">Ser1</span>
```

```
Cities
Chicago 5409916
Houston 4606964
Los Angeles 7952644
NYC 17075346
Philadelhia NaN
Phoenix 3230034
Name: US Populate cities, dtype: float64

```
Name the Index

```
<span class="n">Ser1</span><span class="o">.</span><span class="n">index</span><span class="o">.</span><span class="n">name</span> <span class="o">=</span> <span class="s">'Cities'</span>
<span class="n">Ser1</span>
```

```
Cities
Chicago 5409916
Houston 4606964
Los Angeles 7952644
NYC 17075346
Philadelhia NaN
Phoenix 3230034
Name: US Populate cities, dtype: float64
```

## Dataframes

Dataframe has many functions to read the data from various sources like Json, CSV, Excel, HTML etc. ro dataframe object.Here we would be reading the Salary data from the CSV file

```
<span class="n">df</span><span class="o">=</span><span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="s">"<span style="color: #993300;">/Users/vbabu/Documents/personal/MyGitWork/csv/mls-salaries-2016.csv</span>"</span><span class="p">)</span>
```

Now using the ‘head’ function you can see the first five rows by default and similarly for the bottom 5 rows use ‘tail’ function
```
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
```

|club|last_name|first_name|position|base_salary|guaranteed_compensation|
|--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |
|ATL|Burgos|Efrain|M|62508|62508.0|
|ATL|Oblitey Otoo|Jeffrey|M|51504|51504.0|
|ATL|Tambakis|Alexander|GK|63000|63000.0|
|CHI|Accam|David|F-M|700000|770937.5|
|CHI|Alvarez|Arturo|F|115000|118264.0|

```
<span class="n">df</span><span class="o">.</span><span class="n">tail</span><span class="p">()</span>
```

|club|last_name|first_name|position|base_salary|guaranteed_compensation|
|--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |
|NaN|Martinez|Cristian|M|62508.00|67008.00|
|NaN|Moore|Luke|F|115000.00|137500.00|
|NaN|Nyassi|Sanna|M|135000.00|141250.00|
|NaN|Ovalle|Adolfo|M|63000.00|73500.00|
|NaN|Tshuma|Schillo|F|81999.96|119999.96|

Get the information about the dataframe using the ‘info’ function, Which gives details like field datatype, number of values, memory usage etc.

<class ‘pandas.core.frame.DataFrame’>
Int64Index: 555 entries, 0 to 554
Data columns (total 6 columns):
club 549 non-null object
last_name 555 non-null object
first_name 553 non-null object
position 555 non-null object
base_salary 555 non-null float64
guaranteed_compensation 555 non-null float64
dtypes: float64(2), object(4)
memory usage: 30.4+ KB


Let’s check the data for a single Column ‘last_name”, Using ‘head’ function we will check the first five values for this column

```
<span class="n">df</span><span class="p">[</span><span class="s">'<span style="color: #993300;">last_name</span>'</span><span class="p">]</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
```

```
0 Burgos
1 Oblitey Otoo
2 Tambakis
3 Accam
4 Alvarez
Name: last_name, dtype: object
```

## Add a New Column to Dataframe

Let’s add ‘Age’ column to the existing dataframe, You can see there is no value set for the new column so Pandas handles it internally and set the value as NaN

```
<span class="n">DataFrame</span><span class="p">(</span><span class="n">df</span><span class="p">,</span><span class="n">columns</span><span class="o">=</span><span class="p">[</span><span style="color: #993300;"><span class="s">'last_name'</span><span class="p">,</span><span class="s">'first_name'</span><span class="p">,</span><span class="s">'Age'</span></span><span class="p">])</span>
```

|last_name|first_name|Age|
|--- |--- |--- |
|Burgos|Efrain|NaN|
|Oblitey Otoo|Jeffrey|NaN|
|Tambakis|Alexander|NaN|
|Accam|David|NaN|
|Alvarez|Arturo|NaN|
|Calistri|Joey|NaN|
|Campbell|Jonathan|NaN|
|Cocis|Razvan|NaN|
|Conner|Drew|NaN|
|Doody|Patrick|NaN|
|Fernandez|Collin|NaN|
|Gehrig|Eric|NaN|
|Goossens|John|NaN|
|Harrington|Michael|NaN|
|Igboananike|Kennedy|NaN|
|Johnson|Sean|NaN|
|Junior|Gilberto|NaN|
|Kappelhof|Johan|NaN|
|LaBrocca|Nick|NaN|
|Lampson|Matt|NaN|
|McLain|Patrick|NaN|
|Meira|Joao|NaN|
|Morrell|Alex|NaN|
|Polster|Matt|NaN|
|Ramos|Rodrigo|NaN|
|Stephens|Michael|NaN|
|Thiam|Khaly|NaN|
|Vincent|Brandon|NaN|
|Afful|Harrison|NaN|
|Ashe|Corey|NaN|
|…|…|…|
|Carducci|Marco|NaN|
|Dean|Christian|NaN|
|Flores|Deybi|NaN|
|Froese|Kianz|NaN|
|Harvey|Jordan|NaN|
|Hurtado|Erik|NaN|
|Jacobson|Andrew|NaN|
|Kah|Pa Modou|NaN|
|Kudo|Masato|NaN|
|Laba|Matias|NaN|
|Manneh|Kekuta|NaN|
|McKendry|Ben|NaN|
|Mezquida|Nicolas|NaN|
|Morales|Pedro|NaN|
|Ousted|David|NaN|
|Parker|Tim|NaN|
|Perez|Blas|NaN|
|Rivero|Octavio|NaN|
|Seiler|Cole|NaN|
|Smith|Jordan|NaN|
|Techera|Cristian|NaN|
|Teibert|Russell|NaN|
|Tornaghi|Paolo|NaN|
|Waston|Kendall|NaN|
|Gargan|Dan|NaN|
|Martinez|Cristian|NaN|
|Moore|Luke|NaN|
|Nyassi|Sanna|NaN|
|Ovalle|Adolfo|NaN|
|Tshuma|Schillo|NaN|

555 rows × 3 columns

Let’s see how we can get the value from the index, So here we want to get the 4th row value from the dataframe. All the columns of the 4th row is shown

```
<span class="n">df</span><span class="o">.</span><span class="n">ix</span><span class="p">[</span><span class="mi">4</span><span class="p">]</span>
```

```
club CHI
last_name Alvarez
first_name Arturo
position F
base_salary 115000
guaranteed_compensation 118264
Name: 4, dtype: object
```

Add a new column – base salary * 2

This is another example of adding a new column ‘Double_Salary’ which is double the ‘base_salary’ of existing column.

```
<span class="n">df</span><span class="p">[</span><span class="s">'Double_Salary'</span><span class="p">]</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s">'base_salary'</span><span class="p">]</span><span class="o">*</span><span class="mi">2</span>
```

```
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">3</span><span class="p">)</span>
```

{:.table}
{:.table-responsive}
|club|last_name|first_name|position|base_salary|guaranteed_compensation|Double_Salary|taxes|
|--- |--- |--- |--- |--- |--- |--- |--- |
|ATL|Burgos|Efrain|M|62508.00|62508.00|125016.00|Base Tax|
|ATL|Oblitey Otoo|Jeffrey|M|51504.00|51504.00|103008.00|Base Tax|
|ATL|Tambakis|Alexander|GK|63000.00|63000.00|126000.00|Base Tax|
|CHI|Accam|David|F-M|700000.00|770937.50|1400000.00|Base Tax|
|CHI|Alvarez|Arturo|F|115000.00|118264.00|230000.00|Base Tax|
|CHI|Calistri|Joey|M|51500.00|51500.00|103000.00|Base Tax|
|CHI|Campbell|Jonathan|D|70000.00|78125.00|140000.00|Base Tax|
|CHI|Cocis|Razvan|M|160000.00|160000.00|320000.00|Base Tax|
|CHI|Conner|Drew|M|62500.00|62500.00|125000.00|Base Tax|
|CHI|Doody|Patrick|D|53471.00|53471.00|106942.00|Base Tax|
|CHI|Fernandez|Collin|M|75000.00|77000.00|150000.00|Base Tax|
|CHI|Gehrig|Eric|D|106875.00|112708.33|213750.00|Base Tax|
|CHI|Goossens|John|M|200000.00|200000.00|400000.00|Base Tax|
|CHI|Harrington|Michael|D|125000.00|125000.00|250000.00|Base Tax|
|CHI|Igboananike|Kennedy|F|800000.00|901666.67|1600000.00|Base Tax|
|CHI|Johnson|Sean|GK|250000.00|253000.00|500000.00|Base Tax|
|CHI|Junior|Gilberto|F|1145000.00|1145000.00|2290000.00|Base Tax|
|CHI|Kappelhof|Johan|D|480000.00|520000.00|960000.00|Base Tax|
|CHI|LaBrocca|Nick|M|110000.00|110000.00|220000.00|Base Tax|
|CHI|Lampson|Matt|GK|71250.00|76625.00|142500.00|Base Tax|
|CHI|McLain|Patrick|GK|72500.00|72500.00|145000.00|Base Tax|
|CHI|Meira|Joao|D-M|110000.00|126500.00|220000.00|Base Tax|
|CHI|Morrell|Alex|M|51500.00|51500.00|103000.00|Base Tax|
|CHI|Polster|Matt|D-M|84000.00|99000.00|168000.00|Base Tax|
|CHI|Ramos|Rodrigo|D|80000.00|84000.00|160000.00|Base Tax|
|CHI|Stephens|Michael|M|105000.00|115000.00|210000.00|Base Tax|
|CHI|Thiam|Khaly|M|144000.00|144000.00|288000.00|Base Tax|
|CHI|Vincent|Brandon|D|70000.00|91875.00|140000.00|Base Tax|
|CLB|Afful|Harrison|D|275000.00|291666.67|550000.00|Base Tax|
|CLB|Ashe|Corey|D|95000.00|105500.00|190000.00|Base Tax|
|…|…|…|…|…|…|…|…|
|VAN|Carducci|Marco|GK|63000.00|63000.00|126000.00|Base Tax|
|VAN|Dean|Christian|D|110000.00|191000.00|220000.00|Base Tax|
|VAN|Flores|Deybi|M|62500.00|68385.00|125000.00|Base Tax|
|VAN|Froese|Kianz|M|66000.00|70500.00|132000.00|Base Tax|
|VAN|Harvey|Jordan|D|165000.00|165000.00|330000.00|Base Tax|
|VAN|Hurtado|Erik|F/M|86091.50|121091.50|172183.00|Base Tax|
|VAN|Jacobson|Andrew|M|62500.08|87500.08|125000.16|Base Tax|
|VAN|Kah|Pa Modou|D|97000.00|101850.00|194000.00|Base Tax|
|VAN|Kudo|Masato|F|310000.00|314000.00|620000.00|Base Tax|
|VAN|Laba|Matias|M|560000.00|720500.00|1120000.00|Base Tax|
|VAN|Manneh|Kekuta|M-F|127500.00|157000.00|255000.00|Base Tax|
|VAN|McKendry|Ben|M|52500.00|52500.00|105000.00|Base Tax|
|VAN|Mezquida|Nicolas|M-F|88000.00|88000.00|176000.00|Base Tax|
|VAN|Morales|Pedro|M|1232500.00|1258900.00|2465000.00|Base Tax|
|VAN|Ousted|David|GK|360000.00|378933.33|720000.00|Base Tax|
|VAN|Parker|Tim|D|66000.00|84750.00|132000.00|Base Tax|
|VAN|Perez|Blas|F|215000.00|215000.00|430000.00|Base Tax|
|VAN|Rivero|Octavio|F|890850.00|890850.00|1781700.00|Base Tax|
|VAN|Seiler|Cole|D|51500.00|51500.00|103000.00|Base Tax|
|VAN|Smith|Jordan|D|115000.00|122743.00|230000.00|Base Tax|
|VAN|Techera|Cristian|M|320000.00|345000.00|640000.00|Base Tax|
|VAN|Teibert|Russell|M|115000.00|182500.00|230000.00|Base Tax|
|VAN|Tornaghi|Paolo|GK|62500.00|62500.00|125000.00|Base Tax|
|VAN|Waston|Kendall|D|300000.00|318125.00|600000.00|Base Tax|
|NaN|Gargan|Dan|D|145000.00|145000.00|290000.00|Base Tax|
|NaN|Martinez|Cristian|M|62508.00|67008.00|125016.00|Base Tax|
|NaN|Moore|Luke|F|115000.00|137500.00|230000.00|Base Tax|
|NaN|Nyassi|Sanna|M|135000.00|141250.00|270000.00|Base Tax|
|NaN|Ovalle|Adolfo|M|63000.00|73500.00|126000.00|Base Tax|
|NaN|Tshuma|Schillo|F|81999.96|119999.96|163999.92|Base Tax|


Add specific values to the new column based on the indexes, So here for the indexes 0 & 6, the ‘taxes’ column value would be ‘Federal Tax’ & ‘State Tax’ and rest of the indexes will have the NaN value or null

```
<span class="n">tax</span> <span class="o">=</span> <span class="n">Series</span><span class="p">([</span><span style="color: #993300;"><span class="s">'Federal Tax'</span><span class="p">,</span><span class="s">'State Tax'</span></span><span class="p">],</span><span class="n">index</span><span class="o">=</span><span class="p">[</span><span style="color: #993300;"><span class="mi">0</span><span class="p">,</span><span class="mi">6</span></span><span class="p">])</span>
```

```
<span class="n">df</span><span class="p">[</span><span class="s">'taxes'</span><span class="p">]</span> <span class="o">=</span> <span class="n">tax</span>
```

```
<span class="n">df</span>
```

|club|last_name|first_name|position|base_salary|guaranteed_compensation|Double_Salary|taxes|
|--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |--- |
|ATL|Burgos|Efrain|M|62508.00|62508.00|125016.00|Federal Tax|
|ATL|Oblitey Otoo|Jeffrey|M|51504.00|51504.00|103008.00|NaN|
|ATL|Tambakis|Alexander|GK|63000.00|63000.00|126000.00|NaN|
|CHI|Accam|David|F-M|700000.00|770937.50|1400000.00|NaN|
|CHI|Alvarez|Arturo|F|115000.00|118264.00|230000.00|NaN|
|CHI|Calistri|Joey|M|51500.00|51500.00|103000.00|NaN|
|CHI|Campbell|Jonathan|D|70000.00|78125.00|140000.00|State Tax|
|CHI|Cocis|Razvan|M|160000.00|160000.00|320000.00|NaN|
|CHI|Conner|Drew|M|62500.00|62500.00|125000.00|NaN|
|CHI|Doody|Patrick|D|53471.00|53471.00|106942.00|NaN|
|CHI|Fernandez|Collin|M|75000.00|77000.00|150000.00|NaN|
|CHI|Gehrig|Eric|D|106875.00|112708.33|213750.00|NaN|
|CHI|Goossens|John|M|200000.00|200000.00|400000.00|NaN|
|CHI|Harrington|Michael|D|125000.00|125000.00|250000.00|NaN|
|CHI|Igboananike|Kennedy|F|800000.00|901666.67|1600000.00|NaN|
|CHI|Johnson|Sean|GK|250000.00|253000.00|500000.00|NaN|
|CHI|Junior|Gilberto|F|1145000.00|1145000.00|2290000.00|NaN|
|CHI|Kappelhof|Johan|D|480000.00|520000.00|960000.00|NaN|
|CHI|LaBrocca|Nick|M|110000.00|110000.00|220000.00|NaN|
|CHI|Lampson|Matt|GK|71250.00|76625.00|142500.00|NaN|
|CHI|McLain|Patrick|GK|72500.00|72500.00|145000.00|NaN|
|CHI|Meira|Joao|D-M|110000.00|126500.00|220000.00|NaN|
|CHI|Morrell|Alex|M|51500.00|51500.00|103000.00|NaN|
|CHI|Polster|Matt|D-M|84000.00|99000.00|168000.00|NaN|
|CHI|Ramos|Rodrigo|D|80000.00|84000.00|160000.00|NaN|
|CHI|Stephens|Michael|M|105000.00|115000.00|210000.00|NaN|
|CHI|Thiam|Khaly|M|144000.00|144000.00|288000.00|NaN|
|CHI|Vincent|Brandon|D|70000.00|91875.00|140000.00|NaN|
|CLB|Afful|Harrison|D|275000.00|291666.67|550000.00|NaN|
|CLB|Ashe|Corey|D|95000.00|105500.00|190000.00|NaN|
|VAN|Carducci|Marco|GK|63000.00|63000.00|126000.00|NaN|
|VAN|Dean|Christian|D|110000.00|191000.00|220000.00|NaN|
|VAN|Flores|Deybi|M|62500.00|68385.00|125000.00|NaN|
|VAN|Froese|Kianz|M|66000.00|70500.00|132000.00|NaN|
|VAN|Harvey|Jordan|D|165000.00|165000.00|330000.00|NaN|
|VAN|Hurtado|Erik|F/M|86091.50|121091.50|172183.00|NaN|
|VAN|Jacobson|Andrew|M|62500.08|87500.08|125000.16|NaN|
|VAN|Kah|Pa Modou|D|97000.00|101850.00|194000.00|NaN|
|VAN|Kudo|Masato|F|310000.00|314000.00|620000.00|NaN|
|VAN|Laba|Matias|M|560000.00|720500.00|1120000.00|NaN|
|VAN|Manneh|Kekuta|M-F|127500.00|157000.00|255000.00|NaN|
|VAN|McKendry|Ben|M|52500.00|52500.00|105000.00|NaN|
|VAN|Mezquida|Nicolas|M-F|88000.00|88000.00|176000.00|NaN|
|VAN|Morales|Pedro|M|1232500.00|1258900.00|2465000.00|NaN|
|VAN|Ousted|David|GK|360000.00|378933.33|720000.00|NaN|
|VAN|Parker|Tim|D|66000.00|84750.00|132000.00|NaN|
|VAN|Perez|Blas|F|215000.00|215000.00|430000.00|NaN|
|VAN|Rivero|Octavio|F|890850.00|890850.00|1781700.00|NaN|
|VAN|Seiler|Cole|D|51500.00|51500.00|103000.00|NaN|
|VAN|Smith|Jordan|D|115000.00|122743.00|230000.00|NaN|
|VAN|Techera|Cristian|M|320000.00|345000.00|640000.00|NaN|
|VAN|Teibert|Russell|M|115000.00|182500.00|230000.00|NaN|
|VAN|Tornaghi|Paolo|GK|62500.00|62500.00|125000.00|NaN|
|VAN|Waston|Kendall|D|300000.00|318125.00|600000.00|NaN|
|NaN|Gargan|Dan|D|145000.00|145000.00|290000.00|NaN|
|NaN|Martinez|Cristian|M|62508.00|67008.00|125016.00|NaN|
|NaN|Moore|Luke|F|115000.00|137500.00|230000.00|NaN|
|NaN|Nyassi|Sanna|M|135000.00|141250.00|270000.00|NaN|
|NaN|Ovalle|Adolfo|M|63000.00|73500.00|126000.00|NaN|
|NaN|Tshuma|Schillo|F|81999.96|119999.96|163999.92|NaN|


## Delete Column

‘taxes’ column deleted from the dataframe
```
<span class="k">del</span> <span class="n">df</span><span class="p">[</span><span class="s">'taxes'</span><span class="p">]</span>
```

```
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
```

|club|last_name|first_name|position|base_salary|guaranteed_compensation|Double_Salary|
|--- |--- |--- |--- |--- |--- |--- |
|ATL|Burgos|Efrain|M|62508|62508.0|125016|
|ATL|Oblitey Otoo|Jeffrey|M|51504|51504.0|103008|
|ATL|Tambakis|Alexander|GK|63000|63000.0|126000|
|CHI|Accam|David|F-M|700000|770937.5|1400000|
|CHI|Alvarez|Arturo|F|115000|118264.0|230000|


## Dataframe from Dictionary

```
<span class="n">Myfruits</span> <span class="o">=</span> <span class="p">{</span><span class="s">'fruits'</span><span class="p">:[</span><span style="color: #993300;"><span class="s">'Orange'</span><span class="p">,</span><span class="s">'Mango'</span><span class="p">,</span><span class="s">'Grapes'</span><span class="p">,</span><span class="s">'Guava'</span></span><span class="p">],</span><span class="s">'Price'</span> <span class="p">:[</span><span style="color: #993300;"><span class="mi">20</span><span class="p">,</span><span class="mi">50</span><span class="p">,</span><span class="mi">90</span><span class="p">,</span><span class="mi">100</span></span><span class="p">]}</span>
```

```
<span class="n">dffruits</span><span class="o">=</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">Myfruits</span><span class="p">)</span>
```

```
<span class="n">dffruits</span>
```

|Price|fruits|
|--- |--- |
|20|Orange|
|50|Mango|
|90|Grapes|
|100|Guava|

## Reindexing

Create a new Series for colors as index

```
<span class="n">df</span> <span class="o">=</span> <span class="n">Series</span><span class="p">([</span><span style="color: #993300;"><span class="mi">6</span><span class="p">,</span><span class="mi">4</span><span class="p">,</span><span class="mi">5</span><span class="p">,</span><span class="mi">9</span><span class="p">,</span><span class="mi">7</span></span><span class="p">],</span><span class="n">index</span><span class="o">=</span><span class="p">[</span><span style="color: #993300;"><span class="s">'Red'</span><span class="p">,</span><span class="s">'Blue'</span><span class="p">,</span><span class="s">'Green'</span><span class="p">,</span><span class="s">'Brown'</span><span class="p">,</span><span class="s">'Violet'</span></span><span class="p">])</span>
<span class="n">df</span>
```

```
Red       6
Blue      4
Green     5
Brown     9
Violet    7
dtype: int64
```

Re-index and add a new index ‘Pink’ in the existing Series, Therefore the value of index ‘Pink’ is null

```
<span class="n">df</span><span class="o">.</span><span class="n">reindex</span><span class="p">([</span><span style="color: #993300;"><span class="s">'Red'</span><span class="p">,</span><span class="s">'Blue'</span><span class="p">,</span><span class="s">'Green'</span><span class="p">,</span><span class="s">'Brown'</span><span class="p">,</span><span class="s">'Violet'</span><span class="p">,</span><span class="s">'Pink'</span></span><span class="p">])</span>
```

```
Red        6
Blue       4
Green      5
Brown      9
Violet     7
Pink     NaN
dtype: float64
```

Fill the null values with a default value
```
<span class="n">df</span><span class="o">.</span><span class="n">reindex</span><span class="p">([</span><span style="color: #993300;"><span class="s">'Red'</span><span class="p">,</span><span class="s">'Blue'</span><span class="p">,</span><span class="s">'Green'</span><span class="p">,</span><span class="s">'Brown'</span><span class="p">,</span><span class="s">'Violet'</span><span class="p">,</span><span class="s">'Pink'</span></span><span class="p">],</span><span class="n">fill_value</span><span class="o">=</span><span class="mi">10</span><span class="p">)</span>
```

```
Red        6
Blue       4
Green      5
Brown      9
Violet     7
Pink      10
dtype: int64
```

## Reindexing rows, columns or both

Create a Dataframe with random integers and index value as defined below and columns also as shown

```
<span class="n">df</span> <span class="o">=</span> <span class="n">DataFrame</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">random_integers</span><span class="p">(</span><span class="mi">25</span><span class="p">,</span><span class="n">size</span><span class="o">=</span><span class="mi">25</span><span class="p">)</span><span class="o">.</span><span class="n">reshape</span><span class="p">((</span><span class="mi">5</span><span class="p">,</span><span class="mi">5</span><span class="p">)),</span><span class="n">index</span><span class="o">=</span><span class="p">[</span><span style="color: #993300;"><span class="s">'A'</span><span class="p">,</span><span class="s">'B'</span><span class="p">,</span><span class="s">'D'</span><span class="p">,</span><span class="s">'E'</span><span class="p">,</span><span class="s">'F'</span></span><span class="p">],</span>
                   <span class="n">columns</span><span class="o">=</span><span class="p">[</span><span style="color: #993300;"><span class="s">'col1'</span><span class="p">,</span><span class="s">'col2'</span><span class="p">,</span><span class="s">'col3'</span><span class="p">,</span><span class="s">'col4'</span><span class="p">,</span><span class="s">'col5'</span></span><span class="p">])</span>


<span class="n">df</span>
```

|col1|col2|col3|col4|col5|
|--- |--- |--- |--- |--- |--- |
|A|17|3|16|16|18|
|B|2|24|5|3|15|
|C|20|22|5|21|16|
|D|13|13|10|3|20|
|E|15|18|20|2|2|


## Summing up columns and rows

Sum along the rows by using axis =1

```
<span class="n">df</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span>
```

```
A    70
B    49
D    84
E    59
F    57
dtype: int64
```

Sum along the rows by using axis=0

```
<span class="n">dframe</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">)</span>
```
```
col1    65
col2    60
col3    53
col4    82
col5    77
dtype: int64
```

## Simple Stats

```
<span class="n">dframe</span><span class="o">.</span><span class="n">describe</span><span class="p">()</span>
```

|col1|col2|col3|col4|col5|
|--- |--- |--- |--- |--- |
|5.000000|5|5.000000|5.000000|5.00000|
|13.000000|12|10.600000|16.400000|15.40000|
|8.774964|10|5.549775|5.727128|8.70632|
|1.000000|2|1.000000|9.000000|2.00000|
|8.000000|3|11.000000|15.000000|12.00000|
|16.000000|11|13.000000|16.000000|18.00000|
|16.000000|19|13.000000|17.000000|21.00000|
|24.000000|25|15.000000|25.000000|24.00000|

Find the correlation between each of the variables of the dataframe using ‘corr’ function
```
<span class="n">corr</span><span class="o">=</span><span class="n">dframe</span><span class="o">.</span><span class="n">corr</span><span class="p">()</span>
```

Plot the Correlation heatmap matrix using Seaborn – A Data Visualization Library

```
<span class="kn">import</span> <span class="nn">seaborn</span> <span class="k">as</span> <span class="nn">sns</span>
<span class="o">%</span><span class="k">matplotlib</span> inline
<span class="n">sns</span><span class="o">.</span><span class="n">heatmap</span><span class="p">(</span><span class="n">corr</span><span class="p">)</span>
```

![](data:image/png;base64,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)

Missing Values

```
<span class="n">df</span> <span class="o">=</span> <span class="n">DataFrame</span><span class="p">([[</span><span style="color: #993300;"><span class="mi">5</span><span class="p">,</span><span class="mi">4</span><span class="p">,</span><span class="mi">3</span><span class="p">,</span><span class="mi">2</span><span class="p">,</span><span class="mi">7</span></span><span class="p">],[</span><span style="color: #993300;"><span class="mi">6</span><span class="p">,</span><span class="mi">3</span><span class="p">,</span><span class="n">np</span><span class="o">.</span><span class="n">NaN</span><span class="p">,</span><span class="n">np</span><span class="o">.</span><span class="n">NAN</span><span class="p">,</span><span class="mi">9</span></span><span class="p">],[</span><span style="color: #993300;"><span class="n">np</span><span class="o">.</span><span class="n">NaN</span><span class="p">,</span><span class="mi">3</span><span class="p">,</span><spn class="mi">4</span><span class="p">,</span><span class="mi">2</span><span class="p">,</span><span class="n">np</span><span class="o">.</span><span class="n">NaN</span></span><span class="p">]])</span>

<span class="n">df</span>
```

||0|1|2|3|4|
|--- |--- |--- |--- |--- |-- |
|0|5|4|3|2|7|
|1|6|3|NaN|NaN|9|
|2|NaN|3|4|2|NaN|

drop all the columns where null value is available

```
<span class="n">df1</span><span class="o">=</span><span class="n">df</span><span class="o">.</span><span class="n">dropna</span><span class="p">(</span><span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span>
```
```
<span class="n">df1</span>
```
|1|0|
|--- |--- |
|0|4|
|1|3|
|2|3|

Show rows with atleast ‘thresh’ no. of values, Since all the rows contains minimum three non null values that’s why all rows are displayed
```
<span class="n">df2</span><span class="o">=</span><span class="n">df</span><span class="o">.</span><span class="n">dropna</span><span class="p">(</span><span class="n">thresh</span><span class="o">=</span><span class="mi">3</span><span class="p">)</span>
<span class="n">df2</span>
```

||0|1|2|3|4|
|--- |--- |--- |--- |--- |
|0|5|4|3|2|7|
|1|6|3|NaN|NaN|9|
|2|NaN|3|4|2|NaN|


## fillna

fill nulls with values

```
<span class="n">df</span><span class="o">.</span><span class="n">fillna</span><span class="p">(</span><span class="s">'5'</span><span class="p">)</span>
```

||0|1|2|3|4|
|--- |--- |--- |--- |--- |
|0|5|4|3|2|7|
|1|6|3|5|5|9|
|2|5|3|4|2|5|


## Drop rows

```
<span class="n">dframe</span><span class="o">.</span><span class="n">drop</span><span class="p">(</span><span class="s">'B'</span><span class="p">)</span>
```

|col1|col2|col3|col4|col5|
|--- |--- |--- |--- |--- |
|8|2|1|15|18|
|16|3|11|9|12|
|16|11|13|25|24|
|1|25|13|17|21|


Selecting only 2 columns
```
<span class="n">dframe</span><span class="p">[[</span><span class="s">'col1'</span><span class="p">,</span><span class="s">'col2'</span><span class="p">]]</span>
```

|col1|col2|
|--- |--- |
|8|2|
|24|19|
|16|3|
|16|11|
|1|25|

## Selecting Index

```
<span class="n">dframe</span><span class="o">.</span><span class="n">ix</span><span class="p">[</span><span class="s">'D'</span><span class="p">]</span>
```
```
col1    16
col2     3
col3    11
col4     9
col5    12
Name: D, dtype: int64
```

## Selecting on Condition

```
<span class="n">dframe</span><span class="p">[</span><span class="n">dframe</span><span class="p">[</span><span class="s">'col1'</span><span class="p">]</span><span class="o">&gt;</span><span class="mi">10</span><span class="p">]</span>
```
Out[207]:
col1	col2	col3	col4	col5
B	24	19	15	16	2
D	16	3	11	9	12
E	16	11	13	25	24

## Selecting values

```
<span class="n">df</span><span class="o">=</span><span class="n">Series</span><span class="p">([</span><span style="color: #993300;"><span class="mi">6</span><span class="p">,</span><span class="mi">4</span><span class="p">,</span><span class="mi">3</span><span class="p">,</span><span class="mi">2</span></span><span class="p">],</span><span class="n">index</span><span class="o">=</span><span class="p">[</span><span style="color: #993300;"><span class="s">'A'</span><span class="p">,</span><span class="s">'B'</span><span class="p">,</span><span class="s">'C'</span><span class="p">,</span><span class="s">'D'</span></span><span class="p">])</span>
<span class="n">df</span>
```
```
A    6
B    4
C    3
D    2
dtype: int64

```

Select the Index ‘C’ value
```
<span class="n">df</span><span class="p">[</span><span class="s">'C'</span><span class="p">]</span>
```
```
3
1
3
```

Select First two values

```
<span class="n">df</span><span class="p">[</span><span class="mi">0</span><span class="p">:</span><span class="mi">2</span><span class="p">]</span>
```
```
A    6
B    4
dtype: int64
```
## Sorting

Sort will be done by default on the Indexes, so before sorting index was P,R,S,Q and after sorting it changed to P,Q,R,S
```
<span class="n">df</span><span class="o">=</span><span class="n">Series</span><span class="p">([</span><span style="color: #993300;"><span class="mi">6</span><span class="p">,</span><span class="mi">4</span><span class="p">,</span><span class="mi">2</span><span class="p">,</span><span class="mi">1</span></span><span class="p">],</span><span class="n">index</span><span class="o">=</span><span class="p">[</span><span style="color: #993300;"><span class="s">'P'</span><span class="p">,</span><span class="s">'R'</span><span class="p">,</span><span class="s">'S'</span><span class="p">,</span><span class="s">'Q'</span></span><span class="p">])</span>
<span class="n">df</span>
```

```
P    6
R    4
S    2
Q    1
dtype: int64
```

```
<span class="n">df</span><span class="o">.</span><span class="n">sort_index</span><span class="p">()</span>
```
```
P    6
Q    1
R    4
S    2
dtype: int64
```

Order by values

```
<span class="n">df</span><span class="o">.</span><span class="n">sort_values</span><span class="p">()</span>
```
```
Q    1
S    2
R    4
P    6
dtype: int64
```
```
<span class="n">df</span><span class="o">.</span><span class="n">rank</span><span class="p">()</span>
```
```
P    4
R    3
S    2
Q    1
dtype: float64
```

## MultiIndexing

Here we have first index as 1,2,3 and the second index has a,b for all the first index values, as shown here

```
<span class="n">Sr</span> <span class="o">=</span> <span class="n">Series</span><span class="p">([</span><span style="color: #993300;"><span class="mi">5</span><span class="p">,</span><span class="mi">4</span><span class="p">,</span><span class="mi">3</span><span class="p">,</span><span class="mi">7</span><span class="p">,</span><span class="mi">9</span><span class="p">,</span><span class="mi">6</span></span><span class="p">],</span><span class="n">index</span><span class="o">=</span><span class="p">[[</span><span style="color: #993300;"><span class="mi">1</span><span class="p">,</span><span class="mi">1</span><span class="p">,</span><span class="mi">2</span><span class="p">,</span><span class="mi">2</span><span class="p">,</span><span class="mi">3</span><span class="p">,</span><span class="mi">3</span></span><span class="p">],[</span><span style="color: #993300;"><span class="s">'a'</span><span class="p">,</span><span class="s">'b'</span><span class="p">,</span><span class="s">'a'</span><span class="p">,</span><span class="s">'b'</span><span class="p">,</span><span class="s">'a'</span><span class="p">,</span><span class="s">'b'</span></span><span class="p">]])</span>
<span class="n">Sr</span>
```
```
1  a    5
   b    4
2  a    3
   b    7
3  a    9
   b    6
dtype: int64
```

Verify the Index Levels
```
<span class="n">Sr</span><span class="o">.</span><span class="n">index</span>
```
```
MultiIndex(levels=[[<span style="color: #993300;">1, 2, 3</span>], [<span style="color: #993300;">'a', 'b'</span>]],
           labels=[[<span style="color: #993300;">0, 0, 1, 1, 2, 2</span>], [<span style="color: #993300;">0, 1, 0, 1, 0, 1</span>]])
```

Get Values by first Index

```
<span class="n">Sr</span><span class="p">[</span><span class="mi">3</span><span class="p">]</span>
```
```
a    9
b    6
dtype: int64
```


Get Values by Second Index
```
<span class="n">Sr</span><span class="p">[:,</span><span class="s">'b'</span><span class="p">]</span>
```
```
1    4
2    7
3    6
dtype: int64
```

```
<span class="n">Sr</span><span class="o">.</span><span class="n">unstack</span><span class="p">()</span>
```

|a|b|
|--- |--- |
|5|4|
|3|7|
|9|6|

## Group By

Read a CSV file with the data of Booker Prize Winners from 1996-2016

```
<span class="n">df</span><span class="o">=</span><span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="s">'<span style="color: #993300;">./Desktop/Reports-Delete/Booker-Prize.csv</span>'</span><span class="p">)</span>
```

Get the first 3 rows of the dataset

```
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">3</span><span class="p">)</span>
```

|Year|Author|Title|Genre(s)|Country|Unnamed: 5|Unnamed: 6|
|--- |--- |--- |--- |--- |--- |--- |
|1969|P. H. Newby|Something to Answer For|Novel|United Kingdom|NaN|NaN|
|1970|Bernice Rubens|The Elected Member|Novel|United Kingdom|NaN|NaN|
|1970|J. G. Farrell|Troubles|Novel|United Kingdom\nIreland|NaN|NaN|


Groupby Country, Author & Genre to know who are the Authors won the title more than once and which is the country won the Award maximum number of time
```
<span class="n">df</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span style="color: #993300;"><span class="s">'Country'</span><span class="p">,</span><span class="s">'Author'</span><span class="p">,</span><span class="s">'Genre(s)'</span></span><span class="p">])</span><span class="o">.</span><span class="n">count</span><span class="p">()</span><span class="o">.</span><span class="n">sort_values</span><span class="p">(</span><span class="n">by</span><span class="o">=</span><span class="p">[</span><span class="s">'<span style="color: #993300;">Year</span>'</span><span class="p">],</span><span class="n">ascending</span><span class="o">=</span><span class="p">[</span><span class="k">False</span><span class="p">])</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">5</span><span class="p">)</span>
```

||||Year|Title|
|--- |--- |--- |--- |--- |
|Country|Author|Genre(s)| | |
|Australia|Peter Carey|Historical novel|2|2|
|United Kingdom\nIreland|J. G. Farrell|Novel|2|2|
|United Kingdom|Hilary Mantel|Historical novel|2|2|
|South Africa|J. M. Coetzee|Novel|2|2|
|United Kingdom|Kazuo Ishiguro|Historical novel||1|1|


Pandas is an amazing library for data analysis and I have shown some of the basic stuffs to learn but to get more hands on with the library and it’s functions, Use Varied set of data and practice a lot. The only mantra to learn Pandas and gain expertise is Practice, Practice & Practice
































