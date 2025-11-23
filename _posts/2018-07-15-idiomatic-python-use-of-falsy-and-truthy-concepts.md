---
title: 'Idiomatic Python - Use of Falsy and Truthy Concepts'
date: 2018-12-05
permalink: /posts/2018/12/idiomatic-python-use-of-falsy-and-truthy-concepts/
tags:
  - Python
  - Truthy
  - Falsy
  - Idiomatic Python
  - Code Style
---

Out of many, one reason for python's popularity is the readability. Python has code style guidelines and idioms and these allow future readers of the code to comprehend to the intentions of it. It is highly important that the code is readable and concise. One such important tip is to use falsy and truthy concepts.

It should be at our best interest to avoid direct comparison to True, False, None. As such we should be well known about truthy and falsy concepts.

Truthy refers to the values that shall always be considered true. Similarly falsy refers to the values that shall always be considered false.

An empty sequence such as an empty list [], empty dictionaries, 0 for numeric, None are considered false values or falsy. Almost anything excluding the earlier mentioned are considered truthy.

## An example of a code snippet that is considered bad:

```python
i = 0
if i == 0:
    foo()
    # do something
else:
    bar()
    # do something else

x = True
if x == True:
    foo()
    # do something
else:
    bar()
    # do something else

numbers_list = [1, 2, 3, 4]
if len(numbers_list) > 0:
    foo()
    # do something
```

## An example of code snippet that is considered relatively wise:

```python
i = 0
if not i:
    foo()
    # do something
else:
    bar()
    # do something else

x = True
if x:
    foo()
    # do something
else:
    bar()
    # do something else

numbers_list = [1, 2, 3, 4]
if numbers_list:
    foo()
    # do something
```