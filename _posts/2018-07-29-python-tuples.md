---
title: 'Python Tuples'
date: 2018-07-29
permalink: /posts/2018/07/python-tuples/
tags:
  - Python
  - Data Structures
  - Tuples
  - Programming
---

This is an introductory post about tuples in python. We will see through examples what are tuples, its immutable property, use cases, various operations on it. Rather than a blog, it is a set of examples on tuples in python

## Tuples
It is a sequence of objects in python. Unlike lists, tuple are immutable which means the contents of a tuple can't be changed once assigned. We will see in a bit through example immutable property of tuples.

## Defining a Tuple
Tuples are generally created by enclosing a sequence of objects inside parentheses. "(" and ")"

```python
>>> ip_addresses = ("172.19.56.90", "172.37.57.32", "172.54.21.23")
>>> type(ip_addresses)
<class 'tuple'>
>>>
```

## Defining an empty tuple vs single element tuple vs multi-element tuple
```python
>>> # empty tuple
...
>>> ip_addresses = ()
>>> type(ip_addresses)
<class 'tuple'>
>>>
>>> # multi-element tuple
...
>>> ip_addresses = ("172.19.56.90", "172.37.57.32", "172.54.21.23")
>>> type(ip_addresses)
<class 'tuple'>
>>>
>>> # single element tuple
...
>>> ip_addresses = ("172.19.56.90") # incorrect
>>> type(ip_addresses)
<class 'str'>
>>>
>>> ip_addresses = ("172.19.56.90",)
>>> type(ip_addresses)
<class 'tuple'>
>>>
```

From the code snippet above, the method of defining an empty tuple and multi-element tuple seems obvious. However, what's not obvious is that ip_addresses = ("172.19.56.90") evaluates to a str type instead of a tuple. Although a single element tuple is very rare to come in use, there had to be a way to define it. Hence, a single element tuple should end with a comma "," for the interpreter to evaluate it as a tuple.

## Parentheses is also optional
```python
>>> ip_addresses = "172.19.56.90", "172.37.57.32", "172.54.21.23"
>>> type(ip_addresses)
<class 'tuple'>
>>>
```

It is also optional to have the parentheses to define a tuple. This is possible due to a mechanism called packing which is one of the many useful features of Python.

## Packing and Unpacking
Packing as the name suggest is creating an object by packing multiple other objects to make one compact object. Unpacking on the other hand is the vice-versa such that an object is unpacked and assigned to variables the elements of the tuple.

```python
>>> # packing example
...
>>> ip_addresses = "172.19.56.90", "172.37.57.32", "172.54.21.23"
>>>

>>> # unpacking example
...
>>> ip, ip2, ip3 = ip_addresses
>>> ip
'172.19.56.90'
>>> ip2
'172.37.57.32'
>>> ip3
'172.54.21.23'
>>>
```

## The number of variables on the left should equal the number of elements to be unpacked
```python
>>> ip, ip2 = ip_addresses
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 2)
>>>
>>>
>>> ip, ip2, ip3, ip4 = ip_addresses
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: not enough values to unpack (expected 4, got 3)
>>>
```

## Accessing elements from a tuple through index
```python
>>> ip_addresses = ("172.19.56.90", "172.37.57.32", "172.54.21.23")
>>>
>>> ip_addresses[1]
'172.37.57.32'
>>>
>>> ip_addresses[-1]
'172.54.21.23'
>>>
```

## Looping over elements of a tuple
```python
>>> ip_addresses = ("172.19.56.90", "172.37.57.32", "172.54.21.23")
>>> for each_ip in ip_addresses:
...     print(each_ip)
...
172.19.56.90
172.37.57.32
172.54.21.23
>>>
>>>
```

## Slicing
```python
>>> ip_addresses[1:]
('172.37.57.32', '172.54.21.23')
>>>

>>> ip_addresses[:-1]
('172.19.56.90', '172.37.57.32')
>>>
```

## Tuples are immutable
```python
>>> ip_addresses[0] = "Accidental edit"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>>
```

Tuples are immutable. This avoids accidental data change attempts.

## Concatenation and Multiplication
```python
>>> us_east_ips = ("172.19.56.90", "172.37.57.32", "172.54.21.23")
>>>
>>> us_west_ips = ("172.18.11.22", "172.99.22.3")
>>>
>>> type(us_east_ips)
<class 'tuple'>
>>> type(us_west_ips)
<class 'tuple'>
>>>
>>> all_ips = us_east_ips + us_west_ips
>>> type(all_ips)
<class 'tuple'>
>>> all_ips
('172.19.56.90', '172.37.57.32', '172.54.21.23', '172.18.11.22', '172.99.22.3')
>>>
```

Concatenation of two tuples returns a third tuple which contains the all the contents of the both tuples copied in order.

```python
>>> duplicate_values = ("duplicate",)
>>> type(duplicate_values)
<class 'tuple'>
>>> duplicate_values * 5
('duplicate', 'duplicate', 'duplicate', 'duplicate', 'duplicate')
>>>
```

## Count elements in a tuple
```python
>>> arbitrary_values = ('thetaranights.com', 'python', 'python', 'tutorials')
>>>
>>> arbitrary_values.count('python')
2
>>>
>>>
```

## Find index of an element
```python
>>> arbitrary_values = ('thetaranights.com', 'python', 'python', 'tutorials')
>>>
>>> arbitrary_values.index('tutorials')
3
>>>
```

## Comparison of tuples
Comparison operators work for tuples too. The evaluation starts by comparing the first elements from the either tuples and proceeds on further elements until conclusive.

```python
>>> tup = (7, 14, 20)
>>>
>>> tup2 = (7, 14, 21)
>>>
>>> tup < tup2
True
>>>
>>> tup > tup2
False
>>>
```

For tup < tup2

It compares the first elements from either tuples 7 < 7 which is inconclusive, it then proceeds to comparing 14 < 14, still inconclusive, finally 20 < 21, hence True

Similar is the case for tup > tup2.