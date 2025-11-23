---
title: 'Python Variables'
date: 2018-07-25
permalink: /posts/2018/07/python-variables/
tags:
  - Python
  - Variables
  - Assignment
  - Programming Fundamentals
---

The intentions of this blog is to familiarize with how variables are assigned, the mechanism behind variable assignment, discuss equal status and how almost everything is an object in python, manipulations of objects held by the symbolic names that act as containers and termed as variables.

## Variable Assignment
In Python, you don't really assign a value to a variable. Python stores a reference to an object and the object has a value. Unlike other programming languages, you don't need to declare or define a variable before it can be assigned to a value. Assignment is done via "=" operator.

```python
>>> i = 7
>>> print(i)
7
>>>
```

Once the variable is assigned to a value, it can be used in any other expressions such that the variable will be substituted with the value assigned.

## Changing value
```python
>>> temp_value = 7
>>> print(temp_value)
7
>>> temp_value = 4
>>> print(temp_value)
4
>>> temp_value = "thetaranights.com"
>>> print(temp_value)
>>> thetaranights
>>>
```

## Chained Assignment
Python also supports what's referred to as chained assignment. This allows assigning a value to multiple variables at once.

```python
>>> a = b = c = d = 7
>>> print(a)
7
>>> print(b)
7
>>> print(c)
7
>>> print(d)
7
>>> # Alternatively print all values at once. The above construct is used to make it simple to understand.
>>> print(a, b, c, d)
>>> 4 4 4 4
>>>
```

## Type
Unlike most other programming languages, in Python, a variable can be reassigned to values of different types. In most programming language, it is an obligation to use the value of the same type as when declared due to variables being statically typed.

```python
>>> a = 7
>>> print(a)
7
>>> a = 'thetaranights.com'
>>> print(a)
thetaranights.com
>>>
```

## How does assignment work in Python
One of many primary goals of python was to have an equal status. i.e anything from integers, strings, lists, dictionaries, functions, classes, modules, methods can be assigned to variables, placed in lists, stored in dictionaries, passed as arguments, and so forth. Python is a highly object oriented language. Almost everything in Python is an object.

```python
>>> a = 7
>>> type(a)
<class 'int'>
>>>> id(a)
11033280
>>> print(a)
7
>>>
```

Initially, Python creates an integer object, and creates a reference to the object from the variable name. Although now we can use the value directly from the variable name, it is still the object that holds the value and the variable holds the reference.

## Same Object Reference
```python
>>> a = "thetaranights.com"
>>> b = a
>>>
>>> id(a)
140521889892800
>>> id(b)
140521889892800
>>>
```

From the code snippet above, Python does not create a new object for b. What it does create is a reference to the same object that a points to.

## Garbage Collection
```python
>>> a = "facebook.com"
>>> b = a
>>> a = "github.com/bhishan"
>>> b = "thetaranights.com"
>>>
```

Now, that we re-assigned the reference of the variable a to point to a object "github.com/bhishan", and b points to object "thetaranights.com". We have no references to the earlier value "facebook.com" from either a and b or any other variables. Since it is no longer referenced by any of the variables, it is orphaned. An object's life begins at which time at least one reference to it is created. During an object's lifetime, additional references to it may be created, and references to it may be deleted. An object stays alive, as long as there is at least one reference to it. When the number of references to an object drops to zero, it is no longer accessible. At that point, its lifetime is over. Python will eventually notice that it is inaccessible and reclaim the allocated memory so it can be used for something else. This process is termed as garbage collection.

In technical terms, the id of the object is released when the count of reference to it drops to zero and this id can then be reused. Otherwise, it is guaranteed that no two objects will have the same id. The object id is given by the built-in id() which gives the integer identifier of an object.

## No two objects can have the same id
```python
>>> a = "thetaranights.com"
>>> b = a
>>> id(a)
140521889892800
>>> id(b)
140521889892800
>>> b = "facebook.com"
>>> id(b)
140521889823280
>>>
```

In the above code snippet, we see that the variables a and b initially point to the same object which is verified by the built-in id() that returns the same integer id for both a and b. However, when we re-assign the b to point to another object, the id has now changed. Therefore, no two objects can have the same id at the same lifetime.

## Python caches small integers
Python at startup of the interpreter, creates objects for the integers in the range [-5, 256](inclusive). Every time a variable is assigned a value in this range, it refers to the same object and hence the results from the id() built-in on those variables coincide.

```python
>>> a = -5
>>> b = -5
>>> id(a)
11032896
>>> id(b)
11032896
>>>
```

When we do the same for integers out of this range, a new object is created for each assignment and hence the results from the id() built-in differes.

```python
>>> a = 257
>>> b = 257
>>> id(a)
139653174388272
>>> id(b)
139653174388144
>>>
```

## Naming Convention
A variable name can be of any length and can consist of uppercase letters(A-Z), lowercase letters(a-z), digits(0-9), underscore(_) character and unicode characters(Python3 onwards). Therefore a variable name can be a combination of any of the above with an exception that variable names can't start with digit.

| Valid | Invalid |
|-------|---------|
| address | $address |
| address1 | 1address |
| address_1 | 1_address |
| _ | $ |
| Δ #Greek letter delta | % |

Normally, the intentions of a variable should be understandable at a glance. Hence, a variable name should always be as descriptive as it can be. Often times than not, it is our need to create multi-word variable which should also be readable. Following are some generally used constructs for multi-word variables:

| Construct Name | Description | Example |
|---------------|-------------|---------|
| Camel Case | Second and subsequent words are capitalized. | numberOfStudents |
| Pascal Case | Identical to Camel Case except the first word is also capitalized. | NumberOfStudents |
| Snake Case | Words are separated by underscores | number_of_students |

The style guidelines for Python code, also known as PEP8 recommends using Snake Case for functions and variable names and Pascal Case for class names.