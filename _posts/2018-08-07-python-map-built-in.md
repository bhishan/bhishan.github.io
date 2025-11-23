---
title: 'Python map() built-in'
date: 2018-08-07
permalink: /posts/2018/07/python-map-built-in/
tags:
  - Python
  - Built-in Functions
  - Functional Programming
---

Map makes an iterator that takes a function and uses the arguments from the following iterables passed to the map built-in. What makes this possible is the equal status of every object in Python. One of the main goals of Python was to have an equal status for all the objects. Remember how even a function is an object in Python and hence it can be assigned to a variable, passed as an argument to a function, etc.

```
map(func, *iterables)
```

The first argument is a function that you want each of the elements of the following iterables to be passed as an argument and be evaluated.

Other than the function object, a map built-in should have at least one iterable and could have iterables as an argument such that the arguments for the function is taken from each of the iterables.

## Map takes at least two arguments

```python
>>> def square(x):
...     return x**2
...
>>> map(square)
Traceback (most recent call last):
  File "", line 1, in 
TypeError: map() must have at least two arguments.
>>>
```

## Map Example

```python
>>> def square(x):
...     return x**2
...
>>> squared = map(square, [1,2,3,4,5])
>>> squared
<map object at 0x7f1948bbbef0>
>>> list(squared)
[1, 4, 9, 16, 25]
>>>
```

## Map could take multiple iterables

```python
>>> def add_and_square(x, y):
...     return (x+y)**2
...
>>> added_and_squared = map(add_and_square, [1,2,3,4], [5,6,7,8])
>>> added_and_squared
<map object at 0x7f1948b79518>
>>> list(added_and_squared)
[36, 64, 100, 144]
>>>
```

## When you pass iterables of varying length

```python
>>> def add_and_square(x, y):
...     return (x+y)**2
...
>>> added_and_squared = map(add_and_square, [1,2,3,4], [5,6,7,8, 9])
>>> added_and_squared
<map object at 0x7f1948b795f8>
>>> list(added_and_squared)
[36, 64, 100, 144]
>>>
```

When you pass iterables of varying length to map built-in, it falls back to the minimum length.