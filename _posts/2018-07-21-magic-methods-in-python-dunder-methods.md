---
title: 'Magic Methods in Python - Dunder Methods'
date: 2018-07-21
permalink: /posts/2018/07/magic-methods-in-python-dunder-methods/
tags:
  - Python
  - Magic Methods
  - Dunder Methods
  - Object-Oriented Programming
---

Magic methods are the methods that has two underscores as the prefix and suffix to the method name. These are also called dunder methods which is an adopted name for double underscores(methods with double underscores). __init__, __str__ are some magic methods. These are a set of special methods that could be used to enhance your classes in python.

The dunder methods are also usually used for scenarios like operator overloading and allow you to emulate the behavior of the built-in types. We will start by creating a class, implementing a dunder method or two, see available dunder/magic methods that can be used to enrich the functionality of a custom class.

## Creating a custom String class:
```python
>>> class String:
...     def __init__(self, string):
...         self.string = string
...
>>> string = String("thetaranights.com")
>>> print(string)
<__main__.String object at 0x7fec2fad2400>
>>>
```

Even before we realize, we have made use of one of those many magic methods. The __init__ method is a magic method. __init__ is a method where you'd initialize instance attributes and other init activities. People like to call it a constructor. Think about it for a while, the method already takes the instance (self) as a parameter. Before even __init__ is called a blank object is created. The __init__ method then dynamically initializes each member. Taking self as a parameter means the object is already created before __init__ is called.

Earlier in the blog, we said that magic methods allows us to emulate the behavior of the built-in types. The result from the print(string) doesn't really give us what we would generally want. We can implement a magic method __repr__ to present to the user of the String class a better string representation.

```python
>>> class String:
...     def __init__(self, string):
...         self.string = string
...     def __repr__(self):
...         return "String Object: {string}".format(string=self.string)
...
>>>
>>> string = String("thetaranights.com")
>>> print(string)
String Object: thetaranights.com
>>>
```

In the above code snippet, we have implemented the __repr__ magic method to return a better string representation of our String class's instance.

## Another example of dunder method:
Say we want to get the results from concatenating our custom String object with a string, we would do.

```python
>>> print(string + " Thanks for visiting")

TypeError: unsupported operand type(s) for +: 'String' and 'str'
```

In order for this to work we need to implement the __add__ magic method to our class String.

```python
>>> class String:
...     def __init__(self, string):
...         self.string = string
...     def __repr__(self):
...         return "Object String: {string}".format(string=self.string)
...     def __add__(self, to_concatenate):
...         return self.string + to_concatenate
...
>>>
>>> string = String("thetaranights.com")
>>>
>>> print(string + " thanks for visiting")
thetaranights.com thanks for visiting
>>>
```

Now that we have implemented the __add__ magic method, we can now use the + operator. Following is the list of magic methods available:

## Available Magic Methods

### Binary Operators
| Operator | Method |
|----------|--------|
| + | object.__add__(self, other) |
| - | object.__sub__(self, other) |
| * | object.__mul__(self, other) |
| // | object.__floordiv__(self, other) |
| / | object.__truediv__(self, other) |
| % | object.__mod__(self, other) |
| ** | object.__pow__(self, other[, modulo]) |
| << | object.__lshift__(self, other) |
| >> | object.__rshift__(self, other) |
| & | object.__and__(self, other) |
| ^ | object.__xor__(self, other) |
| \| | object.__or__(self, other) |

### Extended Assignment
| Operator | Method |
|----------|--------|
| += | object.__iadd__(self, other) |
| -= | object.__isub__(self, other) |
| *= | object.__imul__(self, other) |
| /= | object.__idiv__(self, other) |
| //= | object.__ifloordiv__(self, other) |
| %= | object.__imod__(self, other) |
| **= | object.__ipow__(self, other[, modulo]) |
| <<= | object.__ilshift__(self, other) |
| >>= | object.__irshift__(self, other) |
| &= | object.__iand__(self, other) |
| ^= | object.__ixor__(self, other) |
| \|= | object.__ior__(self, other) |

### Unary Operators
| Operator | Method |
|----------|--------|
| - | object.__neg__(self) |
| + | object.__pos__(self) |
| abs() | object.__abs__(self) |
| ~ | object.__invert__(self) |
| complex() | object.__complex__(self) |
| int() | object.__int__(self) |
| long() | object.__long__(self) |
| float() | object.__float__(self) |
| oct() | object.__oct__(self) |
| hex() | object.__hex__(self) |

### Comparison Operators
| Operator | Method |
|----------|--------|
| < | object.__lt__(self, other) |
| <= | object.__le__(self, other) |
| == | object.__eq__(self, other) |
| != | object.__ne__(self, other) |
| >= | object.__ge__(self, other) |
| > | object.__gt__(self, other) |

That's my little introduction to dunder/magic methods in Python. You should also read this article on Debugging with breakpoint in python3.7 https://www.thetaranights.com/debugging-with-breakpoint-in-python3-7/