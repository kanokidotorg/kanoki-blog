---
title: "A primer on Python Regular Expression"
date: "2019-11-29"
categories: [ Python ]
tags: [ Python, Regex ]
---

Regex is a group of characters which helps to find pattern within a string. Regex is used in lot of applications including the search engines, search and for find and replace in text documents

Being a Data Scientist it is good to know regex which is found useful in data cleaning since it helps to identify text of a given pattern and perform operations like replace, count etc.

Every character in a regular expression is either a meta-character or a literal. For example: In regex **b.?**, **b** is a literal and **.** & **?** is a meta-character

## **List of widely used Meta-characters:**

table.customTable { width: 70%; background-color: #FFFFFF; border-collapse: collapse; border-width: 2px; border-color: #7ea8f8; border-style: solid; color: #000000; } <div></div> table.customTable td, table.customTable th { border-width: 2px; border-color: #7ea8f8; border-style: solid; padding: 5px; } <div></div> table.customTable thead { background-color: #7ea8f8; }

Meta-Characters

Description

.

Matches any character except a newline

^

Matches start of the String

$

Matches end of the String

\[\]

Indicate a Set of Characters

\[^\]

Match any character except given

()

capture and group

|

Match either abc or def

\\

Either escapes special characters or denotes special sequence

**Examples**:

table.customTable { width: 70%; background-color: #FFFFFF; border-collapse: collapse; border-width: 2px; border-color: #7ea8f8; border-style: solid; color: #000000; } <div></div> table.customTable td, table.customTable th { border-width: 2px; border-color: #7ea8f8; border-style: solid; padding: 5px; } <div></div> table.customTable thead { background-color: #7ea8f8; }

Regex

String

Results

a.c

asc,apc,b6c,a5c

'asc', 'apc', 'a5c'

(^a.+\\d)

abad6c

'abad6'

(\\d.+b$)

abad6cb

'6cb'

\\d(\[a-z\]+)\\d

197-63abd698-cdef

'abd'

\\w(\[^a-z\]+)\\w

abABCd

'ABC'

(\\w.+) (\\w.+)

My hero

('My', 'hero')

abc | def

124-pdef9-24abc98

'def','abc'

\\\*

hgy\*opt

\*

## **Quantifiers**

A quantifier after a token or group specifies how often that a preceding element is allowed to occur.

The most common quantifiers are the question mark ?, the asterisk \* and the plus sign +

### **List of common Quantifiers**

table.customTable { width: 70%; background-color: #FFFFFF; border-collapse: collapse; border-width: 2px; border-color: #7ea8f8; border-style: solid; color: #000000; } <div></div> table.customTable td, table.customTable th { border-width: 2px; border-color: #7ea8f8; border-style: solid; padding: 5px; } <div></div> table.customTable thead { background-color: #7ea8f8; }

Quantifier

Description

?

Match 0 or One repetitions of preceding RE

\*

Match 0 or more repetitions of preceding RE

+

Match 1 or more repetitions of preceding RE but not Zero. Will not match ac

{n}

Matches exactly n copies of the preceding RE, it's 2 here

{m,n}

Match minimum of m and maximum of n repetitions of preceding RE

**Examples**:

table.customTable { width: 70%; background-color: #FFFFFF; border-collapse: collapse; border-width: 2px; border-color: #7ea8f8; border-style: solid; color: #000000; } <div></div> table.customTable td, table.customTable th { border-width: 2px; border-color: #7ea8f8; border-style: solid; padding: 5px; } <div></div> table.customTable thead { background-color: #7ea8f8; }

Regex

String

Results

ab?c

ac

ac

de\*fg

dfg

dfg

ab+c

abbbc

abbbc

\\d(\[a-z\]+)\\d

197-63abd698-cdef

'abd'

a{2}

24abc98aa0

aa

c{3,4}

aacccbbb

ccc

## **Special Sequence:**

it contains an escape character() followed by the below literals which gives them a special meaning

### **List of commonly used special sequences**

table.customTable { width: 70%; background-color: #FFFFFF; border-collapse: collapse; border-width: 2px; border-color: #7ea8f8; border-style: solid; color: #000000; } <div></div> table.customTable td, table.customTable th { border-width: 2px; border-color: #7ea8f8; border-style: solid; padding: 5px; } <div></div> table.customTable thead { background-color: #7ea8f8; }

Meta-Characters

Description

\\d

Matches decimal digits including \[0-9\]

\\D

Opposite of \\d i.e. \[^0-9\]. Matches any character which is not decimal

\\w

\[A-Za-z0-9\_\]

\\W

Matches any character which is not word character

\\s

Whitespace

\\S

Matches any character not whitespace

\\b

Matches empty string at start or end of a word

\\B

Matches empty string when not at start or end of a word

**Examples:**

table.customTable { width: 70%; background-color: #FFFFFF; border-collapse: collapse; border-width: 2px; border-color: #7ea8f8; border-style: solid; color: #000000; } <div></div> table.customTable td, table.customTable th { border-width: 2px; border-color: #7ea8f8; border-style: solid; padding: 5px; } <div></div> table.customTable thead { background-color: #7ea8f8; }

Regex

String

Results

\\d

Absdd45sd23sd

ac

\\D

dfg

dfg

\\w

abbbc

abbbc

\\W

197-63abd698-cdef

'abd'

\\s

24abc98aa0

aa

\\S

aacccbbb

ccc

\\b

24abc98aa0

aa

\\B

aacccbbb

ccc

## **How to use Regular Expression in Python?**

**First import the regular expression module**

```
import re
```

**Convert Regex Expression into Regex Object using Compile**

Compile function is used to convert a regular expression into a regular expression object. Which can be used with the functions match(), find(), search () etc.

```
import re
regex_obj = re.compile('STRE')
regex_obj
```

**Output:** re.compile(r'stre', re.UNICODE)

**Now use this regular expression object to find pattern in a string**

```
search_str = 'NEWYORK15AVE26STREET'
regex_obj.search(search_str)
```

We used search() function to find the pattern 'STRE' in search\_str i.e. NEWYORK15AVE26STREET and the result is a Match Object

**Output:** <\_sre.SRE\_Match object; span=(14, 18), match='STRE'>

## **Commonly Used Regex methods and attributes in Python**

## **Search** (Regex Extract)

It looks through the entire string and returns a match object where regex match is found or else returns a None

Lets find London in the given string using search() method

```
import re
expr = r'LONDON'
str = 'NEWYORK15AVE26STREET56DOWNINGAVE65 \
      LONDON65MOUNTAINAVE23FIFTYSTREET11 LONDON'
re.search(expr,str)
```

It returns a match object because LONDON is found in the second line. So you have seen search() looks for the pattern in multi-line mode as well.

**Output**: <\_sre.SRE\_Match object; span=(41, 47), match='LONDON'>

Note: span=(41,47) in the match object is the position of the string from 41 thru 46.

## **Match**

This method matches the regular expression at the beginning of String only and returns a Match Object. Unlike Search() it doesn't look for the expression in a new line.

if you run the same regular expression on the string above then it returns a None

We change the expression to 'NEWYORK'

```
import re
expr = r'NEWYORK'
str = 'NEWYORK15AVE26STREET56DOWNINGAVE65 LONDON65MOUNTAINAVE23FIFTYSTREET11'
print(re.match(expr,str))
```

It returns a match object because NEWYORK matches the beginning of the string

**Output**: <\_sre.SRE\_Match object; span=(0, 3), match='NEW'>

## **split**

split is used to split the string using the compiled pattern. if no pattern is passed then space is used.

We are splitting this string '5A123B456C7' on any character between A-Z

```
import re
re.split('[A-Z]+', '5A123B456C7')
```

**Output:** \['5', '123', '456', '7'\]

### **split with flag ignore case**

You can use the re.IGNORECASE flag to ignore the case for splitting, if I re-write the above expression like this it will give the same output

```
re.split('[a-z]+', '5A123B456C7',flags=re.IGNORECASE)
```

**Output:** \['5', '123', '456', '7'\]

### **maxsplit**

You can also specify the maximum number of split that you want for the given string.

We will just pass the maxsplit value as 1 in the above expression

```
re.split('[a-z]+', '5A123B456C7',1,flags=re.IGNORECASE)
```

It just splits the string one time only because we passed the maxsplit value = 1

**Output:** \['5', '123B456C7'\]

## **Findall**

It Returns all non-overlapping matches of pattern in string, as a list of strings

Find all the Characters after a number in the String

```
expr = r'\d([A-Z])'
str = '5A123B456C7'
re.findall(expr,str)
```

The output is A,B and C because A follows 5 and B follows 3 and C follows 6 in the string.

All these characters are returned as a list of strings

**Output:** \['A', 'B', 'C'\]

## **finditer**

it returns an iterable match object.

Get all the characters from the string as an iterator

```
expr = r'\w'
str = '5A123B456C7'
itrobj=re.finditer(expr,str)
itrobj
```

**Output:** <callable\_iterator at 0x255dbf58978>

Now lets iterate through this object and get all the characters of the string

```
 [item.group() for item in itrobj]
```

**Output:** \['5', 'A', '1', '2', '3', 'B', '4', '5', '6', 'C', '7'\]

## **Sub** (Regex replace)

It replaces the matching pattern with the given value starting from left to right

We will replace all characters which are followed by a digit in the given string with the character 'C'

```
expr = r'\d([A-Z])'
str = '5A123B456C7'
re.sub(expr,'C',str)
```

**Output:** 'T12T45T7'

### **Maximum number of pattern occurrences to be replaced**

Now we will use count parameter to define the maximum number of patterns found that needs to be replaced. will pass the count value = 2

```
# sub - replace
expr = r'\d([A-Z])'
str = '5A123B456C7'
re.sub(expr,'T',str, count=2)
```

You can see only the first two characters after a digit is replaced. the third character is not replaced and still it's 'C'

Output: 'T12T456C7'

## S**ubn**

It returns a tuple with the replaced string and the number of replacement that is done

```
expr = r'\d([A-Z])'
str = '5A123B456C7'
re.subn(expr,'T',str,count=2)
```

So in the above case with count=2 it returns **('T12T456C7', 2)**

## **Groups in Python regex**

group returns the subgroups of the match. if there is a single argument then it returns a single value and if there are multiple argument then a tuple of matched strings are returned.

```
expr = r'(\w+) (\w+)'
str = '5A123B 4T56C7'
matchobj=re.match(expr,str)
```

### **Get Entire match subgroups**

```
matchobj.groups()
```

it returns tuple of both the matched string i.e. the entire match subgroups

**Output:** ('5A123B 4T56C7')

### **Get the first matched group**

```
matchobj.group(1)
```

**Output:** '5A123B'

### **Get the Second matched group**

```
matchobj.group(1)
```

**Output:** '4T56C7'

### **Start and End in group**

If you want to know the index of the match group in the string then use start for the starting position index of matched pattern and end for the ending position index

### **Here is an Address**

```
Address = "12 Downing StreetBoulevard,Fischer Ave, Kraigal, PY"
```

We want to Read the Address after stripping Boulevard and using start and end group function

```
addmatch = re.search('Boulevard',Address)
Address[:addmatch.start()] + Address[addmatch.end():]
```

**Output:** '12 Downing Street,Fischer Ave, Kraigal, PY'

## **How to debug a Regular Expression?**

debug function gives the debug statements for the regular expression like their ASCII characters and other details that help you to debug if the regular expression is syntactically correct or not

```
import re
re.compile('stre',re.DEBUG)
```

**Output:**

LITERAL 115
LITERAL 116
LITERAL 114
LITERAL 101
LITERAL 101

```
expr = r'\d{2}(.+)\d{2}'
# str = 'NEWYORK15AVE26STREET'
re.compile(expr,re.DEBUG)
```

**Output**:

MAX\_REPEAT 2 2
  IN
    CATEGORY CATEGORY\_DIGIT
SUBPATTERN 1 0 0
  MAX\_REPEAT 1 MAXREPEAT
    ANY None
MAX\_REPEAT 2 2
  IN
    CATEGORY CATEGORY\_DIGIT

## **How to handle Error in Regular Expression?**

An exception is raised when an invalid regular expression is passed to the function or when aan error occurs during compile.

Here we are passing \\k an invalid sequence and it throws an exception when compiled.

```
# Error in Regex
expr = r'\d(\k)'
str = '5A123B456C7'
try:
    rex=re.compile(expr)
except re.error as e:
    print(e)
```

**Output:** bad escape \\k at position 3

## **Conclusion:**

In this detailed post on regex we have covered the critical python functions associated and used for regular expression.

Here are some of the highlights from this post: that we have touched in this post:

1. What are metachracters, Quantifiers and special sequences in regex
2. How Regex is used in python and what are the commonly used regex methods and functions in python
3. Groups in Python Regex and Debugging a regular expression
4. Error handling in Regular Expression

I would say regex is more of practice and understanding the concept and it's implementation to use it like a pro

* * *
