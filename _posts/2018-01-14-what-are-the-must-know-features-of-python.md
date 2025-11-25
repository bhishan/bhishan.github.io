---
title: 'What are the must know features of python programming language'
date: 2018-01-14
permalink: /posts/2018/01/what-are-the-must-know-features-of-python-programming-language/
tags:
  - Python
  - List Comprehensions
  - Generators
  - Docstrings
  - Args and Kwargs
---

## List comprehensions
One of the major features of python is list comprehension. It is a natural way of creating a new list where each element is the result of some operations applied to each member of another sequence of an iterable. The construct of a list comprehension is such that it consists of brackets containing an expression followed by a for clause then by zero or more for or if clause. List comprehensions always returns a list.

### Simple example

```python
squares = [x**2 for x in range(10)]
```

In a rather real usage scenarios, the expression after the bracket '[' is a call to a method/function.

```python
some_list = [function_name(x) for x in some_iterable]
```

## Generators
Before learning about generators, it is important and essential to understand what an iterable is. Putting it simple, an iterable is an object that can be looped over. Therefore, a list, string, dictionary, file, etc are iterable objects.

A generator is something that simplifies creating iterators. More specifically, a generator is a function that produces a sequence of results instead of a single value.

When a generator function is called, it returns a generator object without even beginning execution of the called function. Now when the next method is called for the first time, the function starts executing until it reaches yeild statement. Hence yeilded value is returned by the next call.

### Sample Example
```python
def gen_example():
    print "Begining of the function"
    for i in range(5):
        print "before yeild"
        yield I
        print "after yeild" 
    print "End of the function"
```

## Generators Expressions
It is the generator version of list comprehension. Everything is same as the list comprehension except that it returns a generator.

### Sample Example

```python
squares = (x**2 for x in range(5))
```

## Docstrings
A docstring is a string literal that occurs as the first statement in a module, function, class, or method definition. Such a docstring becomes the
`__doc__`
special attribute of that object. In fact, every module should have a docstring. Additionally all the functions and classes exported by a module should have a docstrings. Also, the public methods including constructor should also have docstrings.

### Sample Example
```python
def sum_elements(elements):
    """Returns the sum of elements of the passed list"""
    return sum(elements)
```

## *args and **kwargs
*args and *kwargs allow you to pass a variable number of arguments to a function. It is not know before hand about how many arguments can be passed to your function.

*args is used to send a non-keyworded variable length argument list to the function.

```python
def func_with_args(*argv):
    for arg in argv:
        print arg


func_with_args('gopal', 'ramesh', 'paresh')
```

The above code produces the result as follows:

```
gopal
ramesh
paresh
```

On the other hand, **kwargs allows you to pass keyworded variable length of arguments to a function. Below is a basic example

```python
def  func_with_kwargs(**kwargs):
    if kwargs is not None:
        for key, value in kwargs.iteritems():
            print "%s == %s" %(key, value)

func_with_kwargs(customer= "Gopal", salesman = "Ramesh")
```

Following is the output of the above code:

```
customer == Gopal
salesman == Ramesh
```

This is an open-ended article. Please comment below about the features you think should not be missed. Thanks for reading.