---
title: 'Idiomatic Python - Looping Approaches'
date: 2018-07-15
permalink: /posts/2018/07/idiomatic-python-looping-approaches/
tags:
  - Python
  - Looping
  - Iteration
  - Idiomatic Python
---

Python has it's own unique techniques and guidelines for looping. Through this article, I will present a few examples on bad and better approaches on looping. While the end goal can be achieved using both sets of the codes to follow, the purpose is to highlight on the better approaches and encourage it.

## Looping over a range of numbers:
```python
>>> for i in [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]:
...     print i ** 2
...
```

### Better approach when using Python3
```python
>>> for i in range(10):
...     print i ** 2
...
```

### Better approach when using Python2
```python
>>> for i in xrange(10):
...     print i ** 2
...
```

Prior to Python3, range was implemented as a function that returns a list instance. This would mean that a list would be created before looping over it. This brought frictions in the community due to the memory and running complexities that came with it. In Python3, range is implemented with lazy evaluation in that it evaluates the item in the sequence as it furthers on the loop. This eliminates the memory and computation issues of the Python2 range function. It is not that this approach was never present. Python2's xrange is equivalent to Python3 range.

## Looping over a collection:
```python
>>> fruits = ["apple", "mango", "grapes", "banana"]
>>> for i in range(len(fruits)):
...     print(fruits[i])
...
```

### Better Approach:
```python
>>> for fruit in fruits:
...     print(fruit)
...
```

In python list is an iterable and hence implements the __iter__ method that returns an iterator. An iterable is an object that has an __iter__ method which returns an iterator, or which defines a __getitem__ method that can take sequential indexes starting from zero (and raises an IndexError when the indexes are no longer valid). So an iterable is an object that you can get an iterator from.
An iterator is an object with a next (Python 2) or __next__ (Python 3) method.

## Looping Backwards:
```python
>>> fruits = ["apple", "mango", "grapes", "banana"]
>>> for i in range(len(fruits) - 1, -1, -1):
...     print(fruits[i])
...
```

### Better Approach:
```python
>>> for fruit in reversed(fruits):
...     print(fruit)
...
```

## Looping over two collections:
```python
>>> names = ["Hussein", "Mohammed", "Osama"]
>>> fruits = ["Apple", "Mango", "Banana"]
>>>
>>> n = min(len(names), len(fruits))
>>> for i in range(n):
...     print(names[i], " => ", fruits[i])
...
```

### Good Approach:
```python
>>> for name, fruit in zip(names, fruits):
...     print(name, " => ", fruit)
...
```

### Better Approach:
```python
>>> from itertools import izip
>>> for name, fruit in izip(names, fruits):
...     print(name, " => ", fruit)
...
```

zip provides a list while izip provides an iterator instead of a list, hence the name izip (I for iterator). You can argue scenarios where zip would be appropriate but not for the above example.