---
title: 'Idiomatic Python - Writing better Python'
date: 2018-07-19
permalink: /posts/2018/07/idiomatic-python-writing-better-python/
tags:
  - Python
  - Dictionaries
  - Looping
  - Idiomatic Python
---

This is a follow-up post of Idiomatic Python – Looping Approaches. The purpose of the article is to highlight on better code and encourage it.

## Looping over dictionary keys
```python
>>> books_price = {
...     'Clean Code: A Handbook of Agile Software Craftsmanship': 42.17,
...     'The Self-Taught Programmer: The Definitive Guide to Programming Professionally': 15.09,
...     'The Art of Computer Programming, Volumes 1-4A Boxed Set': 174.96
... }
>>> for book in books_price:
...     print(book)
...
```

The above code snippet should not be used for mutating the dictionary. You do not want to change the size of the dictionary while you actively iterate over it.

```python
>>> for book in books_price:
...     if book.startswith('The'):
...             del books_price[book]
... 
Traceback (most recent call last):
  File "", line 1, in 
RuntimeError: dictionary changed size during iteration
>>>
>>> books_price
{'Clean Code: A Handbook of Agile Software Craftsmanship': 42.17, 'The Self-Taught Programmer: The Definitive Guide to Programming Professionally': 15.09}
>>>
```

If by any chance you wrapped the above code around a broad exception, you will then have an inconsistent data. See how in the above example one key value pair has been removed from the books_price dictionary.

## Proper way of mutating dictionary while iterating over it's keys:
```python
>>> books_price = {'Clean Code: A Handbook of Agile Software Craftsmanship': 42.17, 'The Art of Computer Programming, Volumes 1-4A Boxed Set': 174.96, 'The Self-Taught Programmer: The Definitive Guide to Programming Professionally': 15.09}
>>>
>>> for book in books_price.keys():
...     if book.startswith('The'):
...         del books_price[book]
...
>>>
>>> books_price
{'Clean Code: A Handbook of Agile Software Craftsmanship': 42.17}
>>>
```

The books_price.keys() copies all the keys of the books_price dictionary and makes a list. What we are really doing is iterating over the list and modifying the dictionary. This way we avoid trying to mutate the dictionary while actively iterating on it.

Note that in python3 dict.keys() only creates an iterable that provides a dynamic view on the keys of the dictionary while not actually making a new list of keys. This is also known as dictionary view. Therefore you have to explicitly pass the dict.keys() to a list class i.e list(dict.keys())

## Construct a dictionary from sequence pairs
```python
>>> books = [
... 'Clean Code: A Handbook of Agile Software Craftsmanship',
... 'The Art of Computer Programming, Volumes 1-4A Boxed Set',
... 'The Self-Taught Programmer: The Definitive Guide to Programming Professionally']
>>> prices = [42.17, 174.96, 15.09]
>>>
>>> from itertools import izip
>>> books_price = dict(izip(books, prices))
>>> books_price
{'Clean Code: A Handbook of Agile Software Craftsmanship': 42.17, 'The Art of Computer Programming, Volumes 1-4A Boxed Set': 174.96, 'The Self-Taught Programmer: The Definitive Guide to Programming Professionally': 15.09}
>>>
```

You could also use zip instead of izip. zip provides a list while izip provides an iterator instead of a list, hence the name izip (I for iterator). In python3 though, zip is equivalent to izip.

```python
>>> books_price = dict(zip(books, prices))
>>> books_price
{'Clean Code: A Handbook of Agile Software Craftsmanship': 42.17, 'The Art of Computer Programming, Volumes 1-4A Boxed Set': 174.96, 'The Self-Taught Programmer: The Definitive Guide to Programming Professionally': 15.09}
>>>
```

## Looping over dictionary keys and values
```python
>>> for key in books_price:
...     print key, ' => ', books_price[key]
...
Clean Code: A Handbook of Agile Software Craftsmanship  =>  42.17
The Art of Computer Programming, Volumes 1-4A Boxed Set  =>  174.96
The Self-Taught Programmer: The Definitive Guide to Programming Professionally  =>  15.09
>>>
```

While the above code works as expected, it needs to re-hash every key and do a value lookup. OR you could do something like this:

```python
>>> for key, value in books_price.items():
...     print key, " => ", value
...
Clean Code: A Handbook of Agile Software Craftsmanship  =>  42.17
The Art of Computer Programming, Volumes 1-4A Boxed Set  =>  174.96
The Self-Taught Programmer: The Definitive Guide to Programming Professionally  =>  15.09
>>>
```

The problem with the above code is that it creates a huge list of the keys and values. A better approach would be:

```python
>>> for key, value in books_price.iteritems():
...     print key, " => ", value
...
Clean Code: A Handbook of Agile Software Craftsmanship  =>  42.17
The Art of Computer Programming, Volumes 1-4A Boxed Set  =>  174.96
The Self-Taught Programmer: The Definitive Guide to Programming Professionally  =>  15.09
>>>
```

iteritems() returns an iterator instead of list. Note that in python3, iteritems() is not present and python3's items() is equivalent to iteritems() in python2.