---
title: 'Python filter() built-in'
date: 2018-08-08
permalink: /posts/2018/08/python-filter-built-in/
tags:
  - Python
  - Built-in Functions
  - Functional Programming
---

Filter makes an iterator that takes a function and uses the arguments from the following iterable passed to the filter built-in. It returns a filtered iterator which contains only those values for which the function(passed as the first argument to the filter) evaluated truth value. What makes this possible is the equal status of every object in Python. One of the main goals of Python was to have an equal status for all the objects. Remember how even a function is an object in Python and hence it can be assigned to a variable, passed as an argument to an another function, etc.

```
filter(function or None, iterable)
```

The first argument is a function that you want each of the elements of the following iterables to be passed as an argument and be evaluated.

Other than the function object, the filter built-in should have one iterable as an argument such that the arguments for the function is taken from the iterable.

## Filter takes two arguments

```python
>>> def isdivisibleby2(x):
...     if x % 2 == 0:
...         return True
...     return False
...
>>> filter([1,2,3,4])
Traceback (most recent call last):
  File "", line 1, in 
TypeError: filter expected 2 arguments, got 1
>>> filter(isdivisibleby2)
Traceback (most recent call last):
  File "", line 1, in 
TypeError: filter expected 2 arguments, got 1
>>> filter(isdivisibleby2, [1,2,3,4], [5,6,7,8])
Traceback (most recent call last):
  File "", line 1, in 
TypeError: filter expected 2 arguments, got 3
>>>
```

## Filter Example

```python
>>> def isdivisibleby2(x):
...     if x % 2 == 0:
...         return True
...     return False
...
>>> filtered_list = filter(isdivisibleby2, [1, 2, 3, 4])
>>> filtered_list
<filter object at 0x7f04cb644da0>
>>> list(filtered_list)
[2, 4]
>>>
```

## Filter evaluates Truthy and Falsy

Filter built-in returns a filtered iterator which contains only those values for which the function(passed as the first argument to the filter) evaluated truth value(truthy). An empty sequence such as an empty list [], empty dictionaries, 0 for numeric, None are considered false values or falsy. Almost anything excluding the earlier mentioned are considered truthy. You should read this post on Truthy and Falsy concepts in Python. https://www.thetaranights.com/idiomatic-python-use-of-falsy-and-truthy-concepts/

```python
>>> def arbitrary_function(x):
...     return x
...
>>> filtered_list = filter(arbitrary_function, [1, 2, 3, 4])
>>> filtered_list
<filter object at 0x7f04cb5e9550>
>>> list(filtered_list)
[1, 2, 3, 4]
>>>
>>> def arbitrary_function(x):
...     return 0 # any of False, None, [], {}
...
>>> filtered_list = filter(arbitrary_function, [1, 2, 3, 4])
>>> filtered_list
<filter object at 0x7f04cb5e92b0>
>>> list(filtered_list)
[]
>>>
```