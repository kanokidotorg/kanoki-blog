---
title: "Excel Libraries in Python"
date: "2022-09-06"
categories: [ pandas, python]
tags: [ pandas, python]

---

There are so many libraries available in Python language that could help to handle the spreadsheets and let you edit, modify, run formulas and help in data analysis. 

Here we are going to see some of the best  excel libraries in Python that can really make your life easier, If you are dealing with excel, csv and spreadsheets regularly.

### Pandas

![](/images/2022/09/pandas.png)

**Pandas** is a fast, powerful, flexible and easy to use open source data analysis and manipulation tool, built on top of the Python programming language.

Pandas is a software library written for the Python programming language for data manipulation and analysis. In particular, it offers data structures and operations for manipulating numerical tables and time series

[https://github.com/pandas-dev/pandas](https://github.com/pandas-dev/pandas)

### xlwt

It is a library for writing data and formatting information to older Excel files (ie: .xls) compatible with Microsoft Excel versions 95 to 2003, The package itself is pure Python with no dependencies on modules or packages outside the standard Python distribution.

[https://github.com/python-excel/xlwt](https://github.com/python-excel/xlwt)

### pycel

It is a small python library that can translate an Excel spreadsheet into executable python code which can be run independently of Excel.

The python code is based on a graph and uses caching & lazy evaluation to ensure (relatively) fast execution. The graph can be exported and analyzed using tools like [Gephi](http://www.gephi.org/)

[https://github.com/dgorissen/pycel](https://github.com/dgorissen/pycel)

### Xlsxwriter

It is a Python module for writing files in the Excel 2007+ XLSX file format.

XlsxWriter can be used to write text, numbers, formulas and hyperlinks to multiple worksheets and it supports features such as formatting and many more, It supports Python 3.4+ and PyPy3 and uses standard libraries only.

[https://github.com/jmcnamara/XlsxWriter](https://github.com/jmcnamara/XlsxWriter)

### xlwings

![](/images/2022/09/xlwings.png)

It is a [BSD-licensed](http://opensource.org/licenses/BSD-3-Clause) Python library that makes it easy to call Python from Excel and vice versa:

- **Scripting**: Automate/interact with Excel from Python using a syntax that is close to VBA.
- **Macros**: Replace your messy VBA macros with clean and powerful Python code.
- **UDFs**: Write User Defined Functions (UDFs) in Python (Windows only).

**Numpy arrays** and **Pandas Series/DataFrames** are fully supported. xlwings-powered workbooks are easy to distribute and work on **Windows** and **macOS**.

xlwings includes all files in the xlwings package except the `pro` folder, i.e., the `xlwings.pro` subpackage.

[https://github.com/xlwings/xlwings](https://github.com/xlwings/xlwings)

### PyExcelerate

![](/images/2022/09/pyexcelerate.png)

It is a Python for writing Excel-compatible XLSX spreadsheet files, with an emphasis on speed.

[https://github.com/kz26/PyExcelerate](https://github.com/kz26/PyExcelerate)

### xlutils

This package provides a collection of utilities for working with Excel files. Since these utilities may require either or both of the xlrd and xlwt packages, they are collected together here, separate from either package.

[https://github.com/python-excel/xlutils](https://github.com/python-excel/xlutils)

### formulas

![](/images/2022/09/formulas.png)

**formulas** implements an interpreter for Excel formulas, which parses and compile Excel formulas expressions.

Moreover, it compiles Excel workbooks to python and executes without using the Excel COM server. Hence, **Excel is not needed**.

[https://github.com/vinci1it2000/formulas](https://github.com/vinci1it2000/formulas)

### pylightxl

![](/images/2022/09/pylightxl.png)

A light weight, zero dependency (only standard libs used), to the point (no bells and whistles) Microsoft Excel reader/writer python 2.7.18 - 3+ library

[https://github.com/PydPiper/pylightxl](https://github.com/PydPiper/pylightxl)

### nb2xls

![](/images/2022/09/nb2xls.png)

It converts Jupyter notebook to Excel Spreadsheets (xlsx), through a new 'Download As' option or via nbconvert on the command line.

Respects tables such as Pandas DataFrames. Also exports image data such as matplotlib output.

Markdown is supported where possible (some elements still need work).

Input (code) cells are not included in the spreadsheet.

This allows you to share your results with non-programmers such that they can still easily play with the data.

[https://github.com/ideonate/nb2xls](https://github.com/ideonate/nb2xls)

### openpyxl

A Python library to read/write Excel 2010 xlsx/xlsm/xltx/xltm files.

It was born from lack of existing library to read/write natively from Python the Office Open XML format.

All kudos to the PHPExcel team as openpyxl is based on PHPExcel http://www.phpexcel.net/

[https://github.com/theorchard/openpyxl](https://github.com/theorchard/openpyxl)

### Koala

It converts any Excel workbook into a python object that enables on the fly calculation without the need of Excel.

Koala parses an Excel workbook and creates a network of all the cells with their dependencies. It is then possible to change any value of a node and recompute all the depending cells.

You can read more on the origins of Koala [here](https://github.com/vallettea/koala/blob/master/doc/presentation.md). If you are looking for ways to contribute, you can get started [here](https://github.com/vallettea/koala/blob/master/doc/contribute.md).

[https://github.com/vallettea/koala](https://github.com/vallettea/koala)

### xlsxpandasformatter

Deals with the limitations of formatting when using Pandas dataframe and xlsxwriter to export to Excel format.

Provides a helper class that wraps the worksheet, workbook and dataframe objects written by pandas to_excel method using the xlsxwriter engine to allow consistent formatting of cells. A FormatedWorksheet is a helper class that wraps the worksheet, workbook and dataframe objects written by pandas to_excel method using the xlsxwriter engine. It takes care of keeping record of cells format and allows user to apply successively formats to columns, rows and cells. It also provides methods to format group of columns based on column name pattern, and apply special separations between groups of rows.

This class is a quick and dirty workaround to the limitations of formatting in xlsxwriter. It was inspired by the other package [XlsxFormatter](https://github.com/Yoyoyoyoyoyoyo/XlsxFormatter). The latter, however, cannot be used in the case of creating xlsxwriter Worksheet through the Pandas `to_excel` method.

[https://github.com/webermarcolivier/xlsxpandasformatter](https://github.com/webermarcolivier/xlsxpandasformatter)

### xls2slsx

![](/images/2022/09/xls2xlsx.png)

- Convert `.xls` files to `.xlsx` using xlrd and openpyxl.
- Convert `.htm` and `.mht` files containing tables or excel contents to `.xlsx` using beautifulsoup4 and openpyxl.

[https://github.com/snoopyjc/xls2xlsx](https://github.com/snoopyjc/xls2xlsx)

### xltable

![](/images/2022/09/xltable.png)

It is an API for writing tabular data and charts to Excel. It is not a replacement for other Excel writing packages such as xlsxwriter, xlwt or pywin32. Instead it uses those packages as a back end to write the Excel files (or to write to Excel directly in the case of pywin32) and provides a higer level abstraction that allows the programmer to deal with tables of data rather than worry about writing individual cells.

The main feature that makes xltable more useful than just writing the Excel files directly is that it can handle tables with formulas that relate to cells in the workbook without having to know in advance where those tables will be placed on a worksheet

[https://github.com/fkarb/xltable](https://github.com/fkarb/xltable)

### pyexcel

![](/images/2022/09/pyexcel.png)

It provides one application programming interface to read, manipulate and write data in various excel formats.

[https://github.com/pyexcel/pyexcel](https://github.com/pyexcel/pyexcel)

