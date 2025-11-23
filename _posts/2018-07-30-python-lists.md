---
title: 'Python Lists'
date: 2018-07-30
permalink: /posts/2018/12/python-lists/
tags:
  - Python
  - Data Structures
  - Lists
  - Programming
---

The intentions of this article is to host a set of example operations that can be performed around lists, a crucial data structure in Python.

## Lists
In Python, List is an object that contains a sequence of other arbitrary objects. Lists unlike tuples are mutable objects.

## Defining a list
Lists are defined by enclosing a sequence of objects inside square brackets, "[" and "]". A list can contain sequence of mixed data types.

```python
>>> # empty list
...
>>> a = []
>>>
>>> type(a)
<class 'list'>
>>>
>>> # list containing same data types
...
>>>
>>> a = [1, 4, 9, 16]
>>>
>>> type(a)
<class 'list'>
>>>
>>> # list containing different data types
...
>>> a = [1, "python", 7.4, True]
>>>
>>> type(a)
<class 'list'>
>>>
```

## A list can be nested
```python
>>> # nested list
...
>>> a = [[1, 4, 9], ["thetaranights.com", "blog", "python"]]
>>> type(a)
<class 'list'>
>>>
>>>
>>> a = [[1, 4, 9], "thetaranights.com"]
>>>
>>> type(a)
<class 'list'>
>>>
```

## Accessing elements from a list via index
List index is used to access elements of a list. List index starts from 0 and should be an integer.

```python
>>> a = [[1, 4, 9], ["thetaranights.com", "blog", "python"]]
>>> a[0]
[1, 4, 9]
>>> a[0][2]
9
>>> a[1][0]
'thetaranights.com'
>>>
```

## Negative Indexing
Python allows accessing elements from a list via negative indexes such that the last element would be accessed via list_name[-1] and second last element would be accessed via list_name[-2]

```python
>>> a = [1, 4, 9, 16]
>>> a[-1]
16
>>> a[-2]
9
>>> a[-3]
4
>>>
```

## Slicing
```python
>>> a = [1, 4, 9, 16, 25]
>>> a[1:3]
[4, 9]
>>> a[:4]
[1, 4, 9, 16]
>>> a[3:]
[16, 25]
>>> a[:-1]
[1, 4, 9, 16]
>>>
```

## Lists are mutable
```python
>>> a = [1, 4, 9, 16, 25]
>>>
>>> a [0] = "mutable"
>>> a
['mutable', 4, 9, 16, 25]
>>>
>>> a[:2] = [256, 1024] # changing a range of elements of a list to the sequence given in the right of assignment operator
>>> a
[256, 1024, 9, 16, 25]
>>>
```

## Adding elements to an existing list
```python
>>> a = [1, 4, 9, 16, 25]
>>> a.append(36)
>>> a
[1, 4, 9, 16, 25, 36]
>>>
```

## Extending a list with another sequence
```python
>>> a = [1, 4, 9, 16]
>>> a.extend([25, 36, 49])
>>> a
[1, 4, 9, 16, 25, 36, 49]
>>> a.extend((64, 91))
>>> a
[1, 4, 9, 16, 25, 36, 49, 64, 91]
>>>
```

## Concatenation and Multiplication
```python
>>> a = [1, 4, 9, 16]
>>> a + [25, 36, 49]
[1, 4, 9, 16, 25, 36, 49]
>>>

>>> # multiplication
...
>>> a * 7
[1, 4, 9, 16, 1, 4, 9, 16, 1, 4, 9, 16, 1, 4, 9, 16, 1, 4, 9, 16, 1, 4, 9, 16, 1, 4, 9, 16]
>>>
```

## Add items to a list before certain index
```python
>>> # first item to the insert() is the index and the later is the value to insert
...
>>> a = [1, 4, 9, 16, 25]
>>> a.insert(2, "new value")
>>> a
[1, 4, 'new value', 9, 16, 25]
>>>
```

## Various other methods on a list

| method | description | usage |
|--------|-------------|-------|
| append() | Append object to the end of list | L.append(object) |
| clear() | Remove all the items from list | L.clear() |
| copy() | A shallow copy of list | L.copy() |
| count() | Return number of occurrences of value passed as argument to the method | L.count(value) |
| extend() | Extend list by appending elements from the iterable | L.extend(iterable) |
| index() | Return first index of the value | L.index(value, [start, [stop]]) |
| insert() | Insert object before index | L.insert(index, object) |
| pop() | Remove and return item at index (defaults to last) | L.pop([index]) |
| remove() | Remove first occurrence of value | L.remove(value) |
| reverse() | Reverse the list in-place | L.reverse() |
| sort() | Sort in-place | L.sort(key=None, reverse=False) |

## List built-ins

| built-in | description |
|----------|-------------|
| len() | Return the number of elements in a list |
| max() | Returns the largest element in the list |
| min() | Returns the smallest element in the list |
| sorted() | Returns the sorted version of the list. It does not sort the given list itself. |
| sum() | Returns the sum of all the elements of the list |
| all() | Returns true if all the elements of the list evaluate to true (See truthy and falsy concepts) |
| any() | Returns true if any element of the list evaluates to true |
| enumerate() | Returns enumerate object that contains the index and corresponding values of an iterable. |
| list() | Converts an iterable (tuple, string, set, dictionary) to a list. |

## List Comprehension
One of the major features of python is list comprehension. It is a natural way of creating a new list where each element is the result of some operations applied to each member of another sequence of an iterable. The construct of a list comprehension is such that it consists of square brackets containing an expression followed by a for clause then by zero or more for or if clause. List comprehensions always returns a list.

```python
>>> [x ** 2 for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
>>>
```

In a rather real usage scenarios, the expression after the bracket '[' is a call to a method/function.

```python
some_list = [function_name(x) for x in some_iterable]
```