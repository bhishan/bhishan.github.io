---
title: 'Python and Excel'
date: 2018-12-09
permalink: /posts/2018-12/python-and-excel/
tags:
  - python
  - excel
  - openpyxl
  - data-processing
---

I intend to host a set of examples on using python to interact and work with excel files. This article in particular will use openpyxl module in python throughout the examples.

Installing openpyxl
======
I am using python3 throughout the examples, however it should work similarly with python2. As a good development practice, we should segregate dependencies between various projects and therefore is advised to use virtual environment or some sort.

```
python3 -m venv excelenv
source excelenv/bin/activate
pip install openpyxl
```

Open an existing document in python
======
```
>>> import openpyxl
>>> wb = openpyxl.load_workbook('example.xlsx')
>>> wb
<openpyxl.workbook.workbook.Workbook object at 0x7f010dce4240>
>>>
```

Selecting sheets from the workbook/document
======
```
>>> wb.get_sheet_names()
['Sheet1']
>>>
>>> sheet = wb.get_sheet_by_name("Sheet1")
>>> sheet
<Worksheet "Sheet1">
>>>
>>> sheet.max_column
3
>>> sheet.max_row
3
>>> sheet.min_row
1
>>> sheet.min_column
2
>>>
>>> sheet['A1'].value
'thetaranights.com'
>>> sheet['B1'].value
102345
>>> sheet['A2'].value
'thetaranights.com'
>>> sheet['B2'].value
123443
>>>
```

When the cell being accessed doesn't have a value
======
```
>>> a = sheet['A4'].value
>>> a
>>> type(a)
<class 'NoneType'>
>>>
```

OpenPyXL will automatically interpret the types of the values in the cells of the sheet and return them as an object of that type. string , int, dates, etc.

```
>>> type(sheet['A3'].value)
<class 'str'>
>>> type(sheet['B3'].value)
<class 'int'>
```

Accessing cell values using row, column directive
======
Although we can access the values of the cell using alphabetic letters directive, we can also access them using row number and column numbers. An excel row and column starts at 1, not 0.

```
>>> sheet.cell(row=0, column=0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/bhishan-1504/excelinpython/excelenv/lib/python3.6/site-packages/openpyxl/worksheet/worksheet.py", line 296, in cell
    raise ValueError("Row or column values must be at least 1")
ValueError: Row or column values must be at least 1
>>>

>>> sheet.cell(row=1, column=1)
<Cell 'Sheet1'.A1>
>>> sheet.cell(row=1, column=1).value
'thetaranights.com'
>>>
```

Reading values from excel sheet
======
```
>>> wb = openpyxl.load_workbook('example.xlsx')
>>> sheet = wb.get_sheet_by_name("Sheet1")
>>> for each_row in range(sheet.min_row, sheet.max_row + 1):
...     for each_col in range(sheet.min_column, sheet.max_column + 1):
...         print(sheet.cell(row=each_row, column=each_col).value)
...
thetaranights.com
102345
thetaranights.com
123443
thetaranights.com
102234
>>>
```

Creating excel document
======
```
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> wb.get_sheet_names()
['Sheet']
>>> sheet = wb.get_sheet_by_name('Sheet')
>>> sheet
<Worksheet "Sheet">
>>> sheet.title = 'Custom Created worksheet'
>>> wb.get_sheet_names()
['Custom Created worksheet']
>>> sheet
<Worksheet "Custom Created worksheet">
>>> sheet['A1'] = 'thetaranights.com'
>>> sheet['A1'].value
'thetaranights.com'
>>>
>>> sheet.append(['thetaranights.com', '102948']) # adding rows to sheet
>>> wb.save('createdbyopenpyxl.xlsx')
>>>
```