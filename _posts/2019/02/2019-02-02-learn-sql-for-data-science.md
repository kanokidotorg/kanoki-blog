---
title: "Learn SQL for Data Science"
date: "2019-02-02"
categories: [ Data Science ]
tags: [ DataScience, SQL ]
---

SQL is important as it is generally one of the first step needed to get your data from a database or data warehouse. SQL is how you query data from databases, which is where companies store data. It is the language for data querying. You might do some manipulation or more complicated work in a language like Pig or Hive, but you will always use SQL in some capacity.If the data resides on RDBMS in an organisation, then I guess there is no debate that SQL skills are the most important ones. However, even in the world beyond RDBMS (and yes this is where a lot of BIG Data happening in this space) people have not been able to write off the use of SQL. The simplicity of SQL combined by the fact that this is a fourth generation language makes it easier to use and understand (and any background that you have with RDBMS makes the transition easier).

Since SQL is so common, queries and reports you write with SQL, even if using a point-and-click UI, can often be shared among people using different tools. When working with Big Data processing tools, you will use SQL for data preparation and wrangling.The primary use of SQL is in the early phase of an experiment you have a data set. Now you will begin to investigate the data, visualize it, identify the schema, identify prevalence, identify NULLs, missing values, incorrect formatting, etc. SQL allows you to play around with the data sets so you can perform these actions.

Data Cleaning takes lot of time and many of us prefer doing it using SQL and there are lot who feels comfortable doing it in Python or R as well. However I try to clean the data in first pass using SQL and then I use Python to do something more complex which is hard to achieve using SQL or required to write custom functions.

So working with SQL is real fun and there are lot which you can achieve for data cleaning using SQL. Here are some of my favorite MYSQL commands which always comes to my rescue while doing the data cleaning.

**Here are some of the most useful commands in MYSQL for a Data Scientist to Learn:**

**Customer Data Table:**

{:.table}
{:.table-striped}
|No|Name|Address_line1|Address_line2|City|
|--- |--- |--- |--- |--- |
|1|L&M Logistics|703-30711 SIMPSON RD|Suite 301|Bridgewater|
|2|DNM Distirbutors ATTN: George|781 MAIN STREET|NULL|Somerville|
|3|Assan Trails|8451 MOUNTAIN AVENUE|FAWCETT RD|Kansas City|
|4|Home Depot # THD|1748 BROOKSIDE AVENUE|TROON CIRCLE|Norman|
|5|Grasshopper|9140 42ND PARK AVENUE|Bldg# 5|Indianapolis|
|6|Old Navy|WEST GEORGIA ST|NULL|Indianapolis|
|7|Walmart ATTN: Michael Gore|DICKSON AVE|%WALMART|Oklahoma City|

**Regular Expression**

The power of regular expression can be used in SQL Query as well to match a string against a specified pattern. It's a powerful tool which is used in many programming languages including python for pattern matching in text.

In the above Customer table I want to find all the Address in Address_Line1 column which has a street number. Row no. 6 & 7 should not be returned from this Query result as it contains only the Street Name and not the Street Number.

**Find All Address in column** **Address_Line1** **with Street Number:**

> _SELECT * FROM CUSTOMER WHERE ADDRESS_LINE1 REGEXP '^[0-9].'_

{:.table}
{:.table-striped}
|No.|Name|Address_Line1|Address_Line2|City|
|--- |--- |--- |--- |--- |
|1|L&M Logistics|703-30711 SIMPSON RD|Suite 301|Bridgewater|
|2|DNM Distirbutors ATTN: George|781MAIN STREET|NULL|Somerville|
|3|Assan Trails|8451MOUNTAIN AVENUE|FAWCETT RD|Kansas City|
|4|Home Depot # THD|1748BROOKSIDE AVENUE|TROON CIRCLE|Norman|
|5|Grasshopper|914042ND PARK AVENUE|Bldg# 5|Indianapolis|

**Substring_Index**

It returns the substring from str before the count occurence of the delimiter. if the count is positive, everything left of the final delimiter will be returned and if the count is negative, everything right of the final delimiter is returned.

**Find the values after character % in Address_line2**

> SELECT ADDRESS_LINE2, SUBSTRING_INDEX( ADDRESS_LINE2 , '%' , -1) FROM CUSTOMER

The first row in Address_line2 column has a string walmart right of '% ' which is a delimiter in this case and as we choose a negative value as the third param in the substring_index command therefore anything right of this delimiter is returned. if there is no delimiter then the string is returned as it is.

{:.table}
{:.table-striped}
|No.|Address_Line2|Substring_index(address_line2, '%', -1)|
|--- |--- |--- |
|1|%WALMART|WALMART|
|2|BLDG# 5|BLDG# 5|
|3|FAWCETT RD|FAWCETT RD|
|4|Suite 301|Suite 301|
|5|TROON CIRCLE|TROON CIRCLE|

**COALESCE**

it's an excellent command when you want to handle the null values in your data. So basically it returns the first non null value in a list. In this select command I want to replace Address_line2 values with Address_line1 wherever Address_Line2 is null

> SELECT ADDRESS_LINE 1, ADDRESS_LINE 2,coalesce(ADDRESS_LINE2,
> ADDRESS_LINE 1) FROM CUSTOMER

Row No. 2 & 5 has a null value for Address_Line2 and it got replaced with equivalent row value in Address_line1

{:.table}
{:.table-striped}
|No.|Address_line1|Address_line2|Coalesce(Address_line2,Address_Line1)|
|--- |--- |--- |--- |
||703-30711 SIMPSON RD|Suite 301|Suite 301|
||781 MAIN STREET|NULL|781 MAIN STREET|
||8451 MOUNTAIN AVENUE|FAWCETT RD|FAWCETT RD|
||1748 BROOKSIDE AVENUE|TROON CIRCLE|TROON CIRCLE|
||9140 42ND PARK AVENUE|Bldg# 5|Bldg# 5|
||WEST GEORGIA ST|NULL|WEST GEORGIA ST|
||DICKSON AVE|%WALMART|%WALMART|

**CAST & CONVERT**

Converts a value to a specified datatypes. There are various datatypes supported as per their documentation like DATE,DATETIME,CHAR etc. For example a string of time can be converted to datatype TIME like this: SELECT CAST("15:26:20" AS TIME);

CAST and CONVERT do the same thing, except that CONVERT allows more options, such as changing character set with USING

Here I am converting the Address_Line1 value to an unsigned 64 bit integer which returns the Street number from all these Addresses. And if no integer is found in values then it returns Zero. Check Row 6 & 7

> SELECT ADDRESS_LINE1, CAST(ADDRESS_LINE1 as UNSIGNED) FROM CUSTOMER

{:.table}
{:.table-striped}
|No.|Address_Line1|CAST(Address_Line1 As Unsigned)|
|--- |--- |--- |
|1|1748 BROOKSIDE AVENUE|1748|
|2|703-30711 SIMPSON RD|703|
|3|781 MAIN STREET|781|
|4|8451 MOUNTAIN AVENUE|8451|
|5|9140 42ND PARK AVENUE|9140|
|6|DICKSON AVE|0|
|7|WEST GEORGIA ST|0|

**LOCATE**

It returns the position of the first occurrence of the substring within a string. it can be very useful when you want to extract a substring before and after a special character or delimiter . In this case I want to extract everything left of the last underscore in the text "Hello_How_Are_You"

Extracts the left part of the string "Hello_How_Are_You" before the last underscore

> SELECT LEFT("Hello_How_Are_You", CHAR_LENGTH("Hello_How_Are_You") - LOCATE('_', REVERSE("Hello_How_Are_You")));

**Hello_How_Are**




**Group_Concat**

It is used to concatenat string across a group with various options. it returns null if there are no non-null values. Additionally it can be used with DISTINCT, ORDER BY and a SEPARATOR

In this case we are trying to concatenate the id's of all the rows which has a matching Name,City and Street Column values.

{:.table}
{:.table-striped}
|No.|id|Name|Street|City|
|--- |--- |--- |--- |--- |
|1|11245|WALMART|Oakland Avenue|Oklahoma City|
|2|11250|HOME DEPOT|Veterans Ave|Memphis|
|3|13214|WALMART|Oakland Avenue|Oklahoma City|
|4|26798|HOME DEPOT|Veterans Ave|Memphis|

Find all the id's of the duplicate rows for Walmart and Home Depot

> select group_concat(id),Name,street,city from customer group by Name,street,city

In the first column the id's for both the walmart rows (i.e. row no. 1 & 3) and Home Depot rows (i.e. row no. 2 & 4) are concatenated and separated with a comma.

{:.table}
{:.table-striped}
|No|Group_Concat(id)|Name|Street|City|
|--- |--- |--- |--- |--- |
|1|11245,13214|WALMART|Oakland Avenue|Oklahoma City|
|2|11250,26798|HOME DEPOT|Veterans Ave|Memphis|

**Replace**

it is used to replace any character or substring from a string. This is quite helpful when you have to strip the special characters from your data. Here I am replacing the special character hash(#) with a space between "Bldg' and '6'.

> Select REPLACE('2143 Pink Street Bldg#6','#',' ') FROM TABLE

**2143 Pink Street Bldg 6**




**Load Data in File**

This is one of the most useful function you are ever going to use. It is more faster than any other methods out there to upload a csv to a Table, a way faster than the UI tool for csv upload. You should know how your data is contained in the file whether it is comma separated, values are enclosed in quotes and every row is a new line in the document or not.

> LOAD DATA INFILE 'c:/customer.csv' INTO TABLE customer
>
> FIELDS TERMINATED BY ','
>
> ENCLOSED BY '"'
>
> LINES TERMINATED BY 'n'
>
> IGNORE 1
>
> ROWS(Name, Address_Line1, Address_line2, City)

**REGEX_REPLACE**

This is a new command which is supported in MYSQL 8 and is very handy for replacing a pattern match in a string. it takes three optional arguments: **position**: The position in expression from where to start,
**occurence** : which occurence of match to replace. By default replaces all occurence ,
**match_type**: it tells how to perform matching such as case sensitive, case insensitive, multiple line mode etc.

> SELECT REGEXP_REPLACE('HELLO WORLD', 'World', 'JOHN');

**Hello JOHN**

Here the word "WORLD" in text "HELLO WORLD" is replaced by "JOHN".

There are other useful REGEX commands like REGEXP_LIKE(), REGEXP_SUBSTR() etc. which I am not covering here but you can check the MYSQL 8.0 documentation for it.

So you have seen how handy and useful are these SQL commands and it's really a great tool in a Data Scientist toolbox to Cleanse the data or to pull the data from the tables to get an insight and run analytics around it. I usually prefers to clean my data in SQL as much as possible and only when I hit any SQL limitations then I go to Pandas and write few line of codes to achieve that. There are lot of other commands and useful features like JOIN, AGGREGATION which I have not covered in this article since there are lot of posts on those basic stuffs. However in my next blog I will try to find some more useful SQL commands which a data scientist can leverage in his work.

Please write to me at vinaybabu2909@gmail.com or share your feedback here in comments section. I would be happy to hear from you.
