---
title: "Python Read Excel and Insert data to SQL"
date: "2019-01-12"
categories: [ Data Science, Pandas, Python ]
tags: [ CSV, Pandas, Python, SQL ]
---

![](/images/2019/01/image-22.png)

Often we encounter this challenge to deal with multiple csv files and we start looking out for options to import these files to MySQL or PostgresSQL Databases. For many of us this possess a challenge because we don't know a good tool or a way to merge these csv files into one and import it to the SQL table. From my experience I learnt there is no requirement for any additional tool or library or install any packages like csvtoolkit to achieve this. if you have Python, Pandas installed on your computer which I assume will be since you came to this blog searching for a pythonic way of doing it.

In this post, I am going to assume that you have MYSQL/PostgresSQL setups done on your computer and the Table that you want to populate with the csv data is already created and have all the required columns same as the columns present in the csv files.

if Pandas not installed on your computer then install it using pip:

```
pip install pandas
```

I have two csv files which I wanted to merge.These files contains Red and White Wines details of different brands and their respective acidity,sugar,chlorides,pH,alcohol content value


a) Red_Wine.csv : Total Rows: 1599

b) White_Wine.csv : Total Rows: 4898

Note both the files have same no. of columns and headers which is as follows:

fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar','chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density','pH', 'sulphates', 'alcohol', 'quality'

**Import Pandas**

```
import pandas
```

## Import csv files into Pandas Dataframe

### Import first csv into a Dataframe:

We are using these two arguments of Pandas read_csv function, First argument is the path of the file where first csv is located and second argument is for the value separators in the file. In my case it is a semi-colon ";" but for most of the csv files it is comma ',' which is a default value of this argument.

```
df=pd.read_csv("C:/user/vbabu/wine_csv/winequality-red.csv",sep=';')
```

### Import second csv into another Dataframe

```
df1=pd.read_csv("C:/user/vbabu/wine_csv/winequality-white.csv",sep=';')
```

## Merge Dataframes:

So we have imported the csv files into two dataframes and now it's time to merge these two files. We will use Pandas concat function to merge these two files together

```
merged_df = pd.concat([df,df1])
```

**Import Merge Dataframe to MySQL and Postgres Table**

Now we will push the rows of this merged dataframes into a MYSQL or Postgres Table and we have to first establish the connection with the MYSQL server using sqlalchemy.

```
from sqlalchemy import create_engine
engine=create_engine("mysql+mysqldb://root:"+'root'+"@localhost/entityresolution")
```

**For MySQL**

```
engine = create_engine("mysql+mysqldb://your_mysql_username:"+'your_mysql_password'+"@Mysql_server_hostname:your_port/Mysql_databasename")
```

**For Postgres**

```
 engine = create_engine('postgresql+psycopg2://your_mysql_username:your_mysql_password@Mysql_server_hostname:your_port/Mysql_databasename')
```

**Pandas to_sql function**

we will use Pandas to_sql function which will insert these rows of merged dataframe into the MYSQL Table.

```
df.to_sql(name=Your_table_name_in_single_quotes, con=engine, if_exists='append',index=False)
```

Look at the if_exists argument which is important here, there are 3 values for these arguments and if the table already exists::

**fail**: it's a default value. Raise a ValueError.

**replace**: Drop the table before inserting new values.

**append**: Insert new values to the existing table.

I have chosen append since I want to insert new and fresh values to an empty Table

So you have seen how easy and fast it is to Merge and Upload the Data from CSV files to MYSQL or Postgres without the overhead of loading any tools or libraries
