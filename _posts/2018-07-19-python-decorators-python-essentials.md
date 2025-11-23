---
title: 'Python Decorators - Python Essentials'
date: 2018-07-19
permalink: /posts/2018/07/python-decorators/
tags:
  - Python
  - Decorators
  - Functional Programming
  - Advanced Python
---

The intentions of this post is to familiarize the concepts of decorators and encourage it's use. Python allows this special ability to pass a function as an argument to another function that adds some extra behavior to the function passed as argument. These higher order functions that accept function arguments are known as decorators. Passing of functions as argument is possible because functions are first class objects in python.

One of many primary goals of python was to have an equal status. i.e anything from integers, strings, lists, dictionaries, functions, classes, modules, methods can be assigned to variables, placed in lists, stored in dictionaries, passed as arguments, and so forth. With that, it is then possible to have a higher order function that takes another function as argument and extends it's behavior while not actively modifying it.

We will start from defining a function, a nested function, a nested function with another function as an argument, syntatic sugar for ease of decorators.

## Defining a function:
```python
>>> def foo(mixed_case):
...     return mixed_case.upper()
...
>>>
>>> foo("All upper case")
'ALL UPPER CASE'
>>>
```

A function is a first class object that returns a value based on the arguments passed to it. In the above example, it takes a string as an argument and returns the uppercase representation of the given string.

## Defining a nested function:
```python
>>> def foo(mixed_case):
...     def bar():
...         print(mixed_case, " => ", upper_case)
...         upper_case = mixed_case.upper()
...     bar()
...     return upper_case
...
>>>
>>> foo("Subscribe to feeds http://feeds.feedburner.com/thetaranights/NZru")
Subscribe to feeds http://feeds.feedburner.com/thetaranights/NZru  =>  SUBSCRIBE TO FEEDS HTTP://FEEDS.FEEDBURNER.COM/THETARANIGHTS/NZRU
'SUBSCRIBE TO FEEDS HTTP://FEEDS.FEEDBURNER.COM/THETARANIGHTS/NZRU'
>>>
```

The bar() function's scope is only within the foo() function and hence when you call the bar() function from outside of the foo() function, you get NameError exception which makes sense.

```python
>>> bar()
Traceback (most recent call last):
  File "", line 1, in 
NameError: name 'bar' is not defined
>>>
```

## Decorators
```python
>>> def foo(arbitrary_function):
...     print("Going to a bar.")
...     arbitrary_function()
...     print("Returning from a bar.")
...
>>>
```

That's a decorator which extends the functionality of the arbitrary_function() by performing some actions before and after calling the function.

## Using the decorator:
```python
>>># First we define an arbitrary function named bar()
>>> def bar():
...     print("Drinking some beer.")
...
>>>
>>> bar
<function bar at 0x7fed7efdd1b8>
>>>
>>> foo(bar)
Going to a bar.
Drinking some beer.
Returning from a bar.
>>>
```

First we created an arbitrary function named bar() and we verify that it is infact a function and pass it as an argument to the foo() function which is a decorator.

## Another example of a decorator
```python
>>> def foo(arbitrary_function):
...     def wrapper():
...         print("Going to a bar.")
...         arbitrary_function()
...         print("Returning from a bar.")
...     return wrapper
...
>>>
>>> foo(bar)
Going to a bar.
Drinking some beer.
Returning from a bar.
>>>
```

Generally decorators have nested functions within them which performs some operations and calls the functions that was passed as an argument followed by cleaning up operations.

## Syntactic Sugar for decorators
Syntactic sugar in a programming language is a syntax that is designed to make things easy to read and express. As such, @ symbol can be used for simplifying calls to a decorator for a function.

```python
>>> def foo(arbitrary_function):
...     def wrapper():
...         print("Going to a bar.")
...         arbitrary_function()
...         print("Returning from a bar.")
...     return wrapper
...
>>>
>>> @foo
... def bar():
...     print("Drinking some beer.")
...
>>>
>>> bar()
Going to a bar.
Drinking some beer.
Returning from a bar.
>>>
```

All you have to do is write this directive @decorator_function on top of the function definition to be passed to the decorator. Note that you can also assign multiple decorators to a function, each decorator in a line.

When you need to pass arguments to a function that you intent to use decorator on, you have to explicitly add *args and **kwargs to the wrapper function of the decorator, else it will get lost. The arguments will then be passed to the function call from within the body of the wrapper function.

```python
>>> def foo(arbitrary_function):
...     def wrapper(*args, **kwargs):
...         print("Going to a bar.")
...         arbitrary_function(*args, **kwargs)
...         print("Returning from a bar.")
...     return wrapper
...
>>>
>>> @foo
... def bar(drink_type):
...     print("Drinking some " + drink_type)
...
>>>
>>> bar("vodka")
Going to a bar.
Drinking some vodka
Returning from a bar.
>>> bar("beer")
Going to a bar.
Drinking some beer
Returning from a bar.
>>>
```

## An example of a decorator when you need to track the execution time of a function call.
```python
>>> def func_timer(arbitrary_function):
...     def wrapper(*args, **kwargs):
...         t = time.time()
...         arbitrary_function(*args, **kwargs)
...         t2 = time.time()
...         return "Total time for execution => " + str(t2 -t)
...     return wrapper
...
>>>
>>> @func_timer
... def bar(drink_type, bottles=1):
...     for i in range(bottles):
...         print("Drinking " + drink_type + " Bottle number: " + str(i+1))
...
>>>
>>> bar("beer", 10)
Drinking beer Bottle number: 1
Drinking beer Bottle number: 2
Drinking beer Bottle number: 3
Drinking beer Bottle number: 4
Drinking beer Bottle number: 5
Drinking beer Bottle number: 6
Drinking beer Bottle number: 7
Drinking beer Bottle number: 8
Drinking beer Bottle number: 9
Drinking beer Bottle number: 10
'Total time for execution => 0.000279188156128'
>>>
>>> bar("vodka")
Drinking vodka Bottle number: 1
'Total time for execution => 6.29425048828e-05'
>>>
```

The above decorator is used to time the execution of a function call. This sums us my little introduction to decorators.