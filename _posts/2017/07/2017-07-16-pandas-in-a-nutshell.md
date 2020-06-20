---
title: "Pandas in a nutshell"
date: "2017-07-16"
categories: [ Data Science, Python ]
tags: [ Pandas, Python ]
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>

Introduction

Pandas is an open source data analysis library in Python and it is extensively used for Data analysis, Data munging and Cleaning. Pandas has a high performing and user friendly data structures.

What makes Pandas a great choice for data analysis? It is it’s rich and highly performant data structures which are built on top of numpy and leverage it’s fast array operations. In thispost we will see how Pandas can be used efficiently for the data analysis and will explore some of it’s basic operations.
Pandas has two types of data structures:

a) Series – it’s a one dimensional array with indexes, it stores a single column or row of data in a Dataframe

b) Dataframe – it’s a tabular spreadsheet like structure representing rows each of which contains one or multiple columns

First, Let’s do some import

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML"></script>

<script type="text/x-mathjax-config">MathJax.Hub.Config({ tex2jax: { inlineMath: [ ['$','$'], ["\\(","\\)"] ], displayMath: [ ['$$','$$'], ["\\[","\\]"] ], processEscapes: true, processEnvironments: true }, // Center justify equations in code and markdown cells. Elsewhere // we use CSS to left justify single line equations in code cells. displayAlign: 'center', "HTML-CSS": { styles: {'.MathJax_Display': {"margin": 0}}, linebreaks: { automatic: true } } });</script>

In \[39\]:

import pandas as pd
import numpy as np
from pandas import Series, DataFrame

# Series[¶](#Series)

#### Create a Series with the list of values and default indexes[¶](#Create-a-Series-with-the-list-of-values-and-default-indexes)

In \[369\]:

this\=Series(\[6,5,4,9,2\])

#### Check how the Series object looks like[¶](#Check-how-the-Series-object-looks-like)

In \[41\]:

this

Out\[41\]:

0 6 1 5 2 4 3 9 4 2 dtype: int6

In \[42\]:

this.values

Out\[42\]:

array(\[6, 5, 4, 9, 2\])

#### Check the Index for this Series, Pandas generates the indexes automatically and default starts from 0[¶](#Check-the-Index-for-this-Series,-Pandas-generates-the-indexes-automatically-and-default-starts-from-0)

In \[43\]:

this.index

Out\[43\]:

Int64Index(\[0, 1, 2, 3, 4\], dtype='int64')

#### Create a Series of US Cities with their Population and Index is set as the City for the Series, The Object takes the Indexes as defined with their corresponding column values i.e Population[¶](#Create-a-Series-of-US-Cities-with-their-Population-and-Index-is-set-as-the-City-for-the-Series,-The-Object-takes-the-Indexes-as-defined-with-their-corresponding-column-values-i.e-Population)

In \[44\]:

citypopulation\_2016 \= Series(\[8537673,3976322,2704958,2303482,1615017\], index\=\['NYC','Los Angeles','Chicago','Houston','Phoenix'\])

In \[370\]:

citypopulation\_2016

Out\[370\]:

NYC 8537673 Los Angeles 3976322 Chicago 2704958 Houston 2303482 Phoenix 1615017 dtype: int64

# Find Population lesser than 2.5 million[¶](#Find-Population-lesser-than-2.5-million)

#### Find the rows with population lower than 2.5 million[¶](#Find-the-rows-with-population-lower-than-2.5-million)

In \[46\]:

citypopulation\_2016\[citypopulation\_2016<2500000\]

Out\[46\]:

Houston 2303482 Phoenix 1615017 dtype: int64

## Is Chicago in the Series[¶](#Is-Chicago-in-the-Series)

#### Check if Chicago is in the Series or not, it will return True or False boolean value[¶](#Check-if-Chicago-is-in-the-Series-or-not,-it-wil-return-True-or-False-boolean-value)

In \[47\]:

'Chicago' in citypopulation\_2016

Out\[47\]:

True

## Create a list of Index[¶](#Create-a-list-of-Index)

#### Create a list of Indexes with one extra value Philadelphia[¶](#Create-a-list-of-Indexes-with-one-extra-value-Philadelphia)

In \[375\]:

cities \= \['NYC','Los Angeles','Chicago','Houston','Phoenix','Philadelhia'\]

#### Create a Series with this new Indexes, The value for Philadelhia is NaN (Not a Number)[¶](#Create-a-Series-with-this-new-Indexes,-The-value-for-Philadelhia-is-NaN-(Not-a-Number))

In \[377\]:

Ser \= Series(citypopulation\_2016,index\=cities)

In \[378\]:

Ser

Out\[378\]:

NYC 8537673 Los Angeles 3976322 Chicago 2704958 Houston 2303482 Phoenix 1615017 Philadelhia NaN dtype: float64

## Philadelphia not in list that's why it is null[¶](#Philadelphia-not-in-list-that's-why-it-is-null)

#### Check what are the null values in the above Series[¶](#Check-what-are-the-null-values-in-the-above-Series)

In \[380\]:

Ser.isnull()

Out\[380\]:

NYC False Los Angeles False Chicago False Houston False Phoenix False Philadelhia True dtype: bool

##                Adding two series[¶](#Adding-two-series)

#### Add the two Series object Ser & citypopulation\_2016 and store the sum to a new object Ser1[¶](#Add-the-two-Series-object-Ser-&-citypopulation_2016-and-store-the-sum-to-a-new-object-Ser1)

In \[386\]:

Ser1 \= Ser+citypopulation\_2016
Ser1

Out\[386\]:

Chicago 5409916 Houston 4606964 Los Angeles 7952644 NYC 17075346 Philadelhia NaN Phoenix 3230034 dtype: float64

## Name the Series[¶](#Name-the-Series)

In \[393\]:

Ser1.name \= "US Populate cities"
Ser1

Out\[393\]:

Cities Chicago 5409916 Houston 4606964 Los Angeles 7952644 NYC 17075346 Philadelhia NaN Phoenix 3230034 Name: US Populate cities, dtype: float64

## Name the Index[¶](#Name-the-Index)

In \[394\]:

Ser1.index.name \= 'Cities'
Ser1

Out\[394\]:

Cities Chicago 5409916 Houston 4606964 Los Angeles 7952644 NYC 17075346 Philadelhia NaN Phoenix 3230034 Name: US Populate cities, dtype: float64

# Dataframes[¶](#Dataframes)

#### Dataframe has many functions to read the data from various sources like Json, CSV, Excel, HTML etc. ro dataframe object.Here we would be reading the Salary data from the CSV file[¶](#Dataframe-has-many-functions-to-read-the-data-from-various-sources-like-Json,-CSV,-Excel,-HTML-etc.-ro-dataframe-object.Here-we-would-be-reading-the-Salary-data-from-the-CSV-file)

In \[395\]:

df\=pd.read\_csv("/Users/vbabu/Documents/personal/MyGitWork/csv/mls-salaries-2016.csv")

#### Now using the 'head' function you can see the first five rows by default and similarly for the bottom 5 rows use 'tail' function[¶](#Now-using-the-'head'-function-you-can-see-the-first-five-rows-by-default-and-similarly-for-the-bottom-5-rows-use-'tail'-function)

In \[396\]:

df.head()

Out\[396\]:

club

last\_name

first\_name

position

base\_salary

guaranteed\_compensation

0

ATL

Burgos

Efrain

M

62508

62508.0

1

ATL

Oblitey Otoo

Jeffrey

M

51504

51504.0

2

ATL

Tambakis

Alexander

GK

63000

63000.0

3

CHI

Accam

David

F-M

700000

770937.5

4

CHI

Alvarez

Arturo

F

115000

118264.0

In \[397\]:

df.tail()

Out\[397\]:

club

last\_name

first\_name

position

base\_salary

guaranteed\_compensation

550

NaN

Martinez

Cristian

M

62508.00

67008.00

551

NaN

Moore

Luke

F

115000.00

137500.00

552

NaN

Nyassi

Sanna

M

135000.00

141250.00

553

NaN

Ovalle

Adolfo

M

63000.00

73500.00

554

NaN

Tshuma

Schillo

F

81999.96

119999.96

#### Get the information about the dataframe using the 'info' function, Which gives details like field datatype, number of values, memory usage etc.[¶](#Get-the-information-about-the-dataframe-using-the-'info'-function,-Which-gives-details-like-field-datatype,-number-of-values,-memory-usage-etc.)

In \[399\]:

df.info()

<class 'pandas.core.frame.DataFrame'> Int64Index: 555 entries, 0 to 554 Data columns (total 6 columns): club 549 non-null object last\_name 555 non-null object first\_name 553 non-null object position 555 non-null object base\_salary 555 non-null float64 guaranteed\_compensation 555 non-null float64 dtypes: float64(2), object(4) memory usage: 30.4+ KB

#### Let's check the data for a single Column 'last\_name", Using 'head' function we will check the first five values for this column[¶](#Let's-check-the-data-for-a-single-Column-'last_name",-Using-'head'-function-we-will-check-the-first-five-values-for-this-column)

In \[402\]:

df\['last\_name'\].head()

Out\[402\]:

0 Burgos 1 Oblitey Otoo 2 Tambakis 3 Accam 4 Alvarez Name: last\_name, dtype: object

## Add a New Column to Dataframe[¶](#Add-a-New-Column-to-Dataframe)

#### Let's add 'Age' column to the existing dataframe, You can see there is no value set for the new column so Pandas handles it internally and set the value as NaN[¶](#Let's-add-'Age'-column-to-the-existing-dataframe,-You-can-see-there-is-no-value-set-for-the-new-column-so-Pandas-handles-it-internally-and-set-the-value-as-NaN)

In \[403\]:

DataFrame(df,columns\=\['last\_name','first\_name','Age'\])

Out\[403\]:

last\_name

first\_name

Age

0

Burgos

Efrain

NaN

1

Oblitey Otoo

Jeffrey

NaN

2

Tambakis

Alexander

NaN

3

Accam

David

NaN

4

Alvarez

Arturo

NaN

5

Calistri

Joey

NaN

6

Campbell

Jonathan

NaN

7

Cocis

Razvan

NaN

8

Conner

Drew

NaN

9

Doody

Patrick

NaN

10

Fernandez

Collin

NaN

11

Gehrig

Eric

NaN

12

Goossens

John

NaN

13

Harrington

Michael

NaN

14

Igboananike

Kennedy

NaN

15

Johnson

Sean

NaN

16

Junior

Gilberto

NaN

17

Kappelhof

Johan

NaN

18

LaBrocca

Nick

NaN

19

Lampson

Matt

NaN

20

McLain

Patrick

NaN

21

Meira

Joao

NaN

22

Morrell

Alex

NaN

23

Polster

Matt

NaN

24

Ramos

Rodrigo

NaN

25

Stephens

Michael

NaN

26

Thiam

Khaly

NaN

27

Vincent

Brandon

NaN

28

Afful

Harrison

NaN

29

Ashe

Corey

NaN

...

...

...

...

525

Carducci

Marco

NaN

526

Dean

Christian

NaN

527

Flores

Deybi

NaN

528

Froese

Kianz

NaN

529

Harvey

Jordan

NaN

530

Hurtado

Erik

NaN

531

Jacobson

Andrew

NaN

532

Kah

Pa Modou

NaN

533

Kudo

Masato

NaN

534

Laba

Matias

NaN

535

Manneh

Kekuta

NaN

536

McKendry

Ben

NaN

537

Mezquida

Nicolas

NaN

538

Morales

Pedro

NaN

539

Ousted

David

NaN

540

Parker

Tim

NaN

541

Perez

Blas

NaN

542

Rivero

Octavio

NaN

543

Seiler

Cole

NaN

544

Smith

Jordan

NaN

545

Techera

Cristian

NaN

546

Teibert

Russell

NaN

547

Tornaghi

Paolo

NaN

548

Waston

Kendall

NaN

549

Gargan

Dan

NaN

550

Martinez

Cristian

NaN

551

Moore

Luke

NaN

552

Nyassi

Sanna

NaN

553

Ovalle

Adolfo

NaN

554

Tshuma

Schillo

NaN

555 rows × 3 columns

#### Let's see how we can get the value from the index, So here we want to get the 4th row value from the dataframe. All the columns of the 4th row is shown[¶](#Let's-see-how-we-can-get-the-value-from-the-index,-So-here-we-want-to-get-the-4th-row-value-from-the-dataframe.-All-the-columns-of-the-4th-row-is-shown)

In \[404\]:

df.ix\[4\]

Out\[404\]:

club CHI last\_name Alvarez first\_name Arturo position F base\_salary 115000 guaranteed\_compensation 118264 Name: 4, dtype: object

## Add a new column - base salary \* 2[¶](#Add-a-new-column---base-salary-*-2)

#### This is another example of adding a new column 'Double\_Salary' which is double the 'base\_salary' of existing column.[¶](#This-is-another-example-of-adding-a-new-column-'Double_Salary'-which-is-double-the-'base_salary'-of-existing-column.)

In \[405\]:

df\['Double\_Salary'\] \= df\['base\_salary'\]\*2

In \[406\]:

df.head(3)

Out\[406\]:

club

last\_name

first\_name

position

base\_salary

guaranteed\_compensation

Double\_Salary

0

ATL

Burgos

Efrain

M

62508

62508

125016

1

ATL

Oblitey Otoo

Jeffrey

M

51504

51504

103008

2

ATL

Tambakis

Alexander

GK

63000

63000

126000

## Try another example to understand this[¶](#Try-another-example-to-understand-this)

#### Add a new colum taxes with the default value 'Base Tax'[¶](#Add-a-new-colum-taxes-with-the-default-value-'Base-Tax')

In \[431\]:

df\['taxes'\] \= 'Base Tax'

In \[432\]:

df

Out\[432\]:

club

last\_name

first\_name

position

base\_salary

guaranteed\_compensation

Double\_Salary

taxes

0

ATL

Burgos

Efrain

M

62508.00

62508.00

125016.00

Base Tax

1

ATL

Oblitey Otoo

Jeffrey

M

51504.00

51504.00

103008.00

Base Tax

2

ATL

Tambakis

Alexander

GK

63000.00

63000.00

126000.00

Base Tax

3

CHI

Accam

David

F-M

700000.00

770937.50

1400000.00

Base Tax

4

CHI

Alvarez

Arturo

F

115000.00

118264.00

230000.00

Base Tax

5

CHI

Calistri

Joey

M

51500.00

51500.00

103000.00

Base Tax

6

CHI

Campbell

Jonathan

D

70000.00

78125.00

140000.00

Base Tax

7

CHI

Cocis

Razvan

M

160000.00

160000.00

320000.00

Base Tax

8

CHI

Conner

Drew

M

62500.00

62500.00

125000.00

Base Tax

9

CHI

Doody

Patrick

D

53471.00

53471.00

106942.00

Base Tax

10

CHI

Fernandez

Collin

M

75000.00

77000.00

150000.00

Base Tax

11

CHI

Gehrig

Eric

D

106875.00

112708.33

213750.00

Base Tax

12

CHI

Goossens

John

M

200000.00

200000.00

400000.00

Base Tax

13

CHI

Harrington

Michael

D

125000.00

125000.00

250000.00

Base Tax

14

CHI

Igboananike

Kennedy

F

800000.00

901666.67

1600000.00

Base Tax

15

CHI

Johnson

Sean

GK

250000.00

253000.00

500000.00

Base Tax

16

CHI

Junior

Gilberto

F

1145000.00

1145000.00

2290000.00

Base Tax

17

CHI

Kappelhof

Johan

D

480000.00

520000.00

960000.00

Base Tax

18

CHI

LaBrocca

Nick

M

110000.00

110000.00

220000.00

Base Tax

19

CHI

Lampson

Matt

GK

71250.00

76625.00

142500.00

Base Tax

20

CHI

McLain

Patrick

GK

72500.00

72500.00

145000.00

Base Tax

21

CHI

Meira

Joao

D-M

110000.00

126500.00

220000.00

Base Tax

22

CHI

Morrell

Alex

M

51500.00

51500.00

103000.00

Base Tax

23

CHI

Polster

Matt

D-M

84000.00

99000.00

168000.00

Base Tax

24

CHI

Ramos

Rodrigo

D

80000.00

84000.00

160000.00

Base Tax

25

CHI

Stephens

Michael

M

105000.00

115000.00

210000.00

Base Tax

26

CHI

Thiam

Khaly

M

144000.00

144000.00

288000.00

Base Tax

27

CHI

Vincent

Brandon

D

70000.00

91875.00

140000.00

Base Tax

28

CLB

Afful

Harrison

D

275000.00

291666.67

550000.00

Base Tax

29

CLB

Ashe

Corey

D

95000.00

105500.00

190000.00

Base Tax

...

...

...

...

...

...

...

...

...

525

VAN

Carducci

Marco

GK

63000.00

63000.00

126000.00

Base Tax

526

VAN

Dean

Christian

D

110000.00

191000.00

220000.00

Base Tax

527

VAN

Flores

Deybi

M

62500.00

68385.00

125000.00

Base Tax

528

VAN

Froese

Kianz

M

66000.00

70500.00

132000.00

Base Tax

529

VAN

Harvey

Jordan

D

165000.00

165000.00

330000.00

Base Tax

530

VAN

Hurtado

Erik

F/M

86091.50

121091.50

172183.00

Base Tax

531

VAN

Jacobson

Andrew

M

62500.08

87500.08

125000.16

Base Tax

532

VAN

Kah

Pa Modou

D

97000.00

101850.00

194000.00

Base Tax

533

VAN

Kudo

Masato

F

310000.00

314000.00

620000.00

Base Tax

534

VAN

Laba

Matias

M

560000.00

720500.00

1120000.00

Base Tax

535

VAN

Manneh

Kekuta

M-F

127500.00

157000.00

255000.00

Base Tax

536

VAN

McKendry

Ben

M

52500.00

52500.00

105000.00

Base Tax

537

VAN

Mezquida

Nicolas

M-F

88000.00

88000.00

176000.00

Base Tax

538

VAN

Morales

Pedro

M

1232500.00

1258900.00

2465000.00

Base Tax

539

VAN

Ousted

David

GK

360000.00

378933.33

720000.00

Base Tax

540

VAN

Parker

Tim

D

66000.00

84750.00

132000.00

Base Tax

541

VAN

Perez

Blas

F

215000.00

215000.00

430000.00

Base Tax

542

VAN

Rivero

Octavio

F

890850.00

890850.00

1781700.00

Base Tax

543

VAN

Seiler

Cole

D

51500.00

51500.00

103000.00

Base Tax

544

VAN

Smith

Jordan

D

115000.00

122743.00

230000.00

Base Tax

545

VAN

Techera

Cristian

M

320000.00

345000.00

640000.00

Base Tax

546

VAN

Teibert

Russell

M

115000.00

182500.00

230000.00

Base Tax

547

VAN

Tornaghi

Paolo

GK

62500.00

62500.00

125000.00

Base Tax

548

VAN

Waston

Kendall

D

300000.00

318125.00

600000.00

Base Tax

549

NaN

Gargan

Dan

D

145000.00

145000.00

290000.00

Base Tax

550

NaN

Martinez

Cristian

M

62508.00

67008.00

125016.00

Base Tax

551

NaN

Moore

Luke

F

115000.00

137500.00

230000.00

Base Tax

552

NaN

Nyassi

Sanna

M

135000.00

141250.00

270000.00

Base Tax

553

NaN

Ovalle

Adolfo

M

63000.00

73500.00

126000.00

Base Tax

554

NaN

Tshuma

Schillo

F

81999.96

119999.96

163999.92

Base Tax

555 rows × 8 columns

#### Add specific values to the new column based on the indexes, So here for the indexes 0 & 6, the 'taxes' column value would be 'Federal Tax' & 'State Tax' and rest of the indexes will have the NaN value or null[¶](#Add-specific-values-to-the-new-column-based-on-the-indexes,-So-here-for-the-indexes-0-&-6,-the-'taxes'-column-value-would-be-'Federal-Tax'-&-'State-Tax'-and-rest-of-the-indexes-will-have-the-NaN-value-or-null)

In \[435\]:

tax \= Series(\['Federal Tax','State Tax'\],index\=\[0,6\])

In \[436\]:

df\['taxes'\] \= tax

In \[437\]:

df

Out\[437\]:

club

last\_name

first\_name

position

base\_salary

guaranteed\_compensation

Double\_Salary

taxes

0

ATL

Burgos

Efrain

M

62508.00

62508.00

125016.00

Federal Tax

1

ATL

Oblitey Otoo

Jeffrey

M

51504.00

51504.00

103008.00

NaN

2

ATL

Tambakis

Alexander

GK

63000.00

63000.00

126000.00

NaN

3

CHI

Accam

David

F-M

700000.00

770937.50

1400000.00

NaN

4

CHI

Alvarez

Arturo

F

115000.00

118264.00

230000.00

NaN

5

CHI

Calistri

Joey

M

51500.00

51500.00

103000.00

NaN

6

CHI

Campbell

Jonathan

D

70000.00

78125.00

140000.00

State Tax

7

CHI

Cocis

Razvan

M

160000.00

160000.00

320000.00

NaN

8

CHI

Conner

Drew

M

62500.00

62500.00

125000.00

NaN

9

CHI

Doody

Patrick

D

53471.00

53471.00

106942.00

NaN

10

CHI

Fernandez

Collin

M

75000.00

77000.00

150000.00

NaN

11

CHI

Gehrig

Eric

D

106875.00

112708.33

213750.00

NaN

12

CHI

Goossens

John

M

200000.00

200000.00

400000.00

NaN

13

CHI

Harrington

Michael

D

125000.00

125000.00

250000.00

NaN

14

CHI

Igboananike

Kennedy

F

800000.00

901666.67

1600000.00

NaN

15

CHI

Johnson

Sean

GK

250000.00

253000.00

500000.00

NaN

16

CHI

Junior

Gilberto

F

1145000.00

1145000.00

2290000.00

NaN

17

CHI

Kappelhof

Johan

D

480000.00

520000.00

960000.00

NaN

18

CHI

LaBrocca

Nick

M

110000.00

110000.00

220000.00

NaN

19

CHI

Lampson

Matt

GK

71250.00

76625.00

142500.00

NaN

20

CHI

McLain

Patrick

GK

72500.00

72500.00

145000.00

NaN

21

CHI

Meira

Joao

D-M

110000.00

126500.00

220000.00

NaN

22

CHI

Morrell

Alex

M

51500.00

51500.00

103000.00

NaN

23

CHI

Polster

Matt

D-M

84000.00

99000.00

168000.00

NaN

24

CHI

Ramos

Rodrigo

D

80000.00

84000.00

160000.00

NaN

25

CHI

Stephens

Michael

M

105000.00

115000.00

210000.00

NaN

26

CHI

Thiam

Khaly

M

144000.00

144000.00

288000.00

NaN

27

CHI

Vincent

Brandon

D

70000.00

91875.00

140000.00

NaN

28

CLB

Afful

Harrison

D

275000.00

291666.67

550000.00

NaN

29

CLB

Ashe

Corey

D

95000.00

105500.00

190000.00

NaN

...

...

...

...

...

...

...

...

...

525

VAN

Carducci

Marco

GK

63000.00

63000.00

126000.00

NaN

526

VAN

Dean

Christian

D

110000.00

191000.00

220000.00

NaN

527

VAN

Flores

Deybi

M

62500.00

68385.00

125000.00

NaN

528

VAN

Froese

Kianz

M

66000.00

70500.00

132000.00

NaN

529

VAN

Harvey

Jordan

D

165000.00

165000.00

330000.00

NaN

530

VAN

Hurtado

Erik

F/M

86091.50

121091.50

172183.00

NaN

531

VAN

Jacobson

Andrew

M

62500.08

87500.08

125000.16

NaN

532

VAN

Kah

Pa Modou

D

97000.00

101850.00

194000.00

NaN

533

VAN

Kudo

Masato

F

310000.00

314000.00

620000.00

NaN

534

VAN

Laba

Matias

M

560000.00

720500.00

1120000.00

NaN

535

VAN

Manneh

Kekuta

M-F

127500.00

157000.00

255000.00

NaN

536

VAN

McKendry

Ben

M

52500.00

52500.00

105000.00

NaN

537

VAN

Mezquida

Nicolas

M-F

88000.00

88000.00

176000.00

NaN

538

VAN

Morales

Pedro

M

1232500.00

1258900.00

2465000.00

NaN

539

VAN

Ousted

David

GK

360000.00

378933.33

720000.00

NaN

540

VAN

Parker

Tim

D

66000.00

84750.00

132000.00

NaN

541

VAN

Perez

Blas

F

215000.00

215000.00

430000.00

NaN

542

VAN

Rivero

Octavio

F

890850.00

890850.00

1781700.00

NaN

543

VAN

Seiler

Cole

D

51500.00

51500.00

103000.00

NaN

544

VAN

Smith

Jordan

D

115000.00

122743.00

230000.00

NaN

545

VAN

Techera

Cristian

M

320000.00

345000.00

640000.00

NaN

546

VAN

Teibert

Russell

M

115000.00

182500.00

230000.00

NaN

547

VAN

Tornaghi

Paolo

GK

62500.00

62500.00

125000.00

NaN

548

VAN

Waston

Kendall

D

300000.00

318125.00

600000.00

NaN

549

NaN

Gargan

Dan

D

145000.00

145000.00

290000.00

NaN

550

NaN

Martinez

Cristian

M

62508.00

67008.00

125016.00

NaN

551

NaN

Moore

Luke

F

115000.00

137500.00

230000.00

NaN

552

NaN

Nyassi

Sanna

M

135000.00

141250.00

270000.00

NaN

553

NaN

Ovalle

Adolfo

M

63000.00

73500.00

126000.00

NaN

554

NaN

Tshuma

Schillo

F

81999.96

119999.96

163999.92

NaN

555 rows × 8 columns

## Delete Column[¶](#Delete-Column)

#### 'taxes' column deleted from the dataframe[¶](#'taxes'-column-deleted-from-the-dataframe)

In \[438\]:

del df\['taxes'\]

In \[439\]:

df.head()

Out\[439\]:

club

last\_name

first\_name

position

base\_salary

guaranteed\_compensation

Double\_Salary

0

ATL

Burgos

Efrain

M

62508

62508.0

125016

1

ATL

Oblitey Otoo

Jeffrey

M

51504

51504.0

103008

2

ATL

Tambakis

Alexander

GK

63000

63000.0

126000

3

CHI

Accam

David

F-M

700000

770937.5

1400000

4

CHI

Alvarez

Arturo

F

115000

118264.0

230000

## Dataframe from Dictionary[¶](#Dataframe-from-Dictionary)

In \[170\]:

Myfruits \= {'fruits':\['Orange','Mango','Grapes','Guava'\],'Price' :\[20,50,90,100\]}

In \[171\]:

dffruits\=DataFrame(Myfruits)

In \[172\]:

dffruits

Out\[172\]:

Price

fruits

0

20

Orange

1

50

Mango

2

90

Grapes

3

100

Guava

# Reindexing[¶](#Reindexing)

#### Create a new Series for colors as index[¶](#Create-a-new-Series-for-colors-as-index)

In \[440\]:

df \= Series(\[6,4,5,9,7\],index\=\['Red','Blue','Green','Brown','Violet'\])
df

Out\[440\]:

Red       6
Blue      4
Green     5
Brown     9
Violet    7
dtype: int64

#### Re-index and add a new index 'Pink' in the existing Series, Therefore the value of index 'Pink' is null[¶](#Re-index-and-add-a-new-index-'Pink'-in-the-existing-Series,-Therefore-the-value-of-index-'Pink'-is-null)

In \[175\]:

df.reindex(\['Red','Blue','Green','Brown','Violet','Pink'\])

Out\[175\]:

Red        6
Blue       4
Green      5
Brown      9
Violet     7
Pink     NaN
dtype: float64

#### Fill the null values with a default value[¶](#Fill-the-null-values-with-a-default-value)

In \[177\]:

df.reindex(\['Red','Blue','Green','Brown','Violet','Pink'\],fill\_value\=10)

Out\[177\]:

Red        6
Blue       4
Green      5
Brown      9
Violet     7
Pink      10
dtype: int64

### Reindexing rows, columns or both[¶](#Reindexing-rows,-columns-or-both)

#### Create a Dataframe with random integers and index value as defined below and columns also as shown[¶](#Create-a-Dataframe-with-random-integers-and-index-value-as-defined-below-and-columns-also-as-shown)

In \[443\]:

df \= DataFrame(np.random.random\_integers(25,size\=25).reshape((5,5)),index\=\['A','B','D','E','F'\],
                   columns\=\['col1','col2','col3','col4','col5'\])

df

Out\[443\]:

col1

col2

col3

col4

col5

A

17

3

16

16

18

B

2

24

5

3

15

D

20

22

5

21

16

E

13

13

10

3

20

F

15

18

20

2

2

# Summing up columns and rows[¶](#Summing-up-columns-and-rows)

#### Sum along the rows by using axis =1[¶](#Sum-along-the-rows-by-using-axis-=1)

In \[446\]:

df.sum(axis\=1)

Out\[446\]:

A    70
B    49
D    84
E    59
F    57
dtype: int64

#### Sum along the rows by using axis=0[¶](#Sum-along-the-rows-by-using-axis=0)

In \[227\]:

dframe.sum(axis\=0)

Out\[227\]:

col1    65
col2    60
col3    53
col4    82
col5    77
dtype: int64

# Simple Stats[¶](#Simple-Stats)

In \[229\]:

dframe.describe()

Out\[229\]:

col1

col2

col3

col4

col5

count

5.000000

5

5.000000

5.000000

5.00000

mean

13.000000

12

10.600000

16.400000

15.40000

std

8.774964

10

5.549775

5.727128

8.70632

min

1.000000

2

1.000000

9.000000

2.00000

25%

8.000000

3

11.000000

15.000000

12.00000

50%

16.000000

11

13.000000

16.000000

18.00000

75%

16.000000

19

13.000000

17.000000

21.00000

max

24.000000

25

15.000000

25.000000

24.00000

#### Find the correlation between each of the variables of the dataframe using 'corr' function[¶](#Find-the-correlation-between-each-of-the-variables-of-the-dataframe-using-'corr'-function)

In \[447\]:

corr\=dframe.corr()

#### Plot the Correlation heatmap matrix using Seaborn - A Data Visualization Library[¶](#Plot-the-Correlation-heatmap-matrix-using-Seaborn---A-Data-Visualization-Library)

In \[448\]:

import seaborn as sns
%matplotlib inline
sns.heatmap(corr)

Out\[448\]:

<matplotlib.axes.\_subplots.AxesSubplot at 0x117841978>

![](data:image/png;base64,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)

# Missing Values[¶](#Missing-Values)

In \[450\]:

df \= DataFrame(\[\[5,4,3,2,7\],\[6,3,np.NaN,np.NAN,9\],\[np.NaN,3,4,2,np.NaN\]\])

df

Out\[450\]:

0

1

2

3

4

0

5

4

3

2

7

1

6

3

NaN

NaN

9

2

NaN

3

4

2

NaN

#### drop all the columns where null value is available[¶](#drop-all-the-columns-where-null-value-is-available)

In \[451\]:

df1\=df.dropna(axis\=1)

In \[255\]:

df1

Out\[255\]:

1

0

4

1

3

2

3

#### Show rows with atleast 'thresh' no. of values, Since all the rows contains minimum three non null values that's why all rows are displayed[¶](#Show-rows-with-atleast-'thresh'-no.-of-values,-Since-all-the-rows-contains-minimum-three-non-null-values-that's-why-all-rows-are-displayed)

In \[461\]:

df2\=df.dropna(thresh\=3)
df2

Out\[461\]:

0

1

2

3

4

0

5

4

3

2

7

1

6

3

NaN

NaN

9

2

NaN

3

4

2

NaN

## fillna[¶](#fillna)

### fill nulls with values[¶](#fill-nulls-with-values)

In \[261\]:

df.fillna('5')

Out\[261\]:

0

1

2

3

4

0

5

4

3

2

7

1

6

3

5

5

9

2

5

3

4

2

5

# Drop rows[¶](#Drop-rows)

In \[193\]:

dframe.drop('B')

Out\[193\]:

col1

col2

col3

col4

col5

A

8

2

1

15

18

D

16

3

11

9

12

E

16

11

13

25

24

F

1

25

13

17

21

# Selecting only 2 columns[¶](#Selecting-only-2-columns)

In \[203\]:

dframe\[\['col1','col2'\]\]

Out\[203\]:

col1

col2

A

8

2

B

24

19

D

16

3

E

16

11

F

1

25

# Selecting Index[¶](#Selecting-Index)

In \[210\]:

dframe.ix\['D'\]

Out\[210\]:

col1    16
col2     3
col3    11
col4     9
col5    12
Name: D, dtype: int64

# Selecting on Condition[¶](#Selecting-on-Condition)

In \[207\]:

dframe\[dframe\['col1'\]\>10\]

Out\[207\]:

col1

col2

col3

col4

col5

B

24

19

15

16

2

D

16

3

11

9

12

E

16

11

13

25

24

# Selecting values[¶](#Selecting-values)

In \[462\]:

df\=Series(\[6,4,3,2\],index\=\['A','B','C','D'\])
df

Out\[462\]:

A    6
B    4
C    3
D    2
dtype: int64

#### Select the Index 'C' value[¶](#Select-the-Index-'C'-value)

In \[464\]:

df\['C'\]

Out\[464\]:

3

# Select First two values[¶](#Select-First-two-values)

In \[202\]:

df\[0:2\]

Out\[202\]:

A    6
B    4
dtype: int64

# Sorting[¶](#Sorting)

#### Sort will be done by default on the Indexes, so before sorting index was P,R,S,Q and after sorting it changed to P,Q,R,S[¶](#Sort-will-be-done-by-default-on-the-Indexes,-so-before-sorting-index-was-P,R,S,Q-and-after-sorting-it-changed-to-P,Q,R,S)

In \[467\]:

df\=Series(\[6,4,2,1\],index\=\['P','R','S','Q'\])
df

Out\[467\]:

P    6
R    4
S    2
Q    1
dtype: int64

In \[468\]:

df.sort\_index()

Out\[468\]:

P    6
Q    1
R    4
S    2
dtype: int64

# Order by values[¶](#Order-by-values)

In \[218\]:

df.sort\_values()

Out\[218\]:

Q    1
S    2
R    4
P    6
dtype: int64

In \[221\]:

df.rank()

Out\[221\]:

P    4
R    3
S    2
Q    1
dtype: float64

# MultiIndexing[¶](#MultiIndexing)

#### Here we have first index as 1,2,3 and the second index has a,b for all the first index values, as shown here[¶](#Here-we-have-first-index-as-1,2,3-and-the-second-index-has-a,b-for-all-the-first-index-values,-as-shown-here)

In \[469\]:

Sr \= Series(\[5,4,3,7,9,6\],index\=\[\[1,1,2,2,3,3\],\['a','b','a','b','a','b'\]\])
Sr

Out\[469\]:

1  a    5
   b    4
2  a    3
   b    7
3  a    9
   b    6
dtype: int64

#### Verify the Index Levels[¶](#Verify-the-Index-Levels)

In \[283\]:

Sr.index

Out\[283\]:

MultiIndex(levels=\[\[1, 2, 3\], \['a', 'b'\]\],
           labels=\[\[0, 0, 1, 1, 2, 2\], \[0, 1, 0, 1, 0, 1\]\])

#### Get Values by first Index[¶](#Get-Values-by-first-Index)

In \[285\]:

Sr\[3\]

Out\[285\]:

a    9
b    6
dtype: int64

#### Get Values by Second Index[¶](#Get-Values-by-Second-Index)

In \[287\]:

Sr\[:,'b'\]

Out\[287\]:

1    4
2    7
3    6
dtype: int64

In \[288\]:

Sr.unstack()

Out\[288\]:

a

b

1

5

4

2

3

7

3

9

6

# Group By[¶](#Group-By)

#### Read a CSV file with the data of Booker Prize Winners from 1996-2016[¶](#Read-a-CSV-file-with-the-data-of-Booker-Prize-Winners-from-1996-2016)

In \[470\]:

df\=pd.read\_csv('./Desktop/Reports-Delete/Booker-Prize.csv')

#### Get the first 3 rows of the dataset[¶](#Get-the-first-3-rows-of-the-dataset)

In \[471\]:

df.head(3)

Out\[471\]:

Year

Author

Title

Genre(s)

Country

Unnamed: 5

Unnamed: 6

0

1969

P. H. Newby

Something to Answer For

Novel

United Kingdom

NaN

NaN

1

1970

Bernice Rubens

The Elected Member

Novel

United Kingdom

NaN

NaN

2

1970

J. G. Farrell

Troubles

Novel

United Kingdom\\nIreland

NaN

NaN

#### Groupby Country, Author & Genre to know who are the Authors won the title more than once and which is the country won the Award maximum number of time[¶](#Groupby-Country,-Author-&-Genre-to-know-who-are-the-Authors-won-the-title-more-than-once-and-which-is-the-country-won-the-Award-maximum-number-of-time)

In \[368\]:

df.groupby(\['Country','Author','Genre(s)'\]).count().sort\_values(by\=\['Year'\],ascending\=\[False\]).head(5)

Out\[368\]:

Year

Title

Country

Author

Genre(s)

Australia

Peter Carey

Historical novel

2

2

United Kingdom\\nIreland

J. G. Farrell

Novel

2

2

United Kingdom

Hilary Mantel

Historical novel

2

2

South Africa

J. M. Coetzee

Novel

2

2

United Kingdom

Kazuo Ishiguro

Historical novel

1

1

#### Pandas is an amazing library for data analysis and I have shown some of the basic stuffs to learn but to get more hands on with the library and it's functions, Use Varied set of data and practice a lot. The only mantra to learn Pandas and gain expertise is Practice, Practice & Practice[¶](#Pandas-is-an-amazing-library-for-data-analysis-and-I-have-shown-some-of-the-basic-stuffs-to-learn-but-to-get-more-hands-on-with-the-library-and-it's-functions,-Use-Varied-set-of-data-and-practice-a-lot.-The-only-mantra-to-learn-Pandas-and-gain-expertise-is-Practice,-Practice-&-Practice)
