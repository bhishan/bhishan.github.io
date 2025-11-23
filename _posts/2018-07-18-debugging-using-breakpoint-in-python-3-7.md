---
title: 'Debugging using breakpoint in Python 3.7'
date: 2018-07-18
permalink: /posts/2018/07/debugging-using-breakpoint-in-python-3-7/
tags:
  - Python
  - Debugging
  - Python 3.7
  - PDB
---

Python has long had a default debugger named pdb in the standard libraries. pdb defines an interactive source code debugger for python programs. The intentions of this post is to clarify through examples and explanations what's with the new built-in breakpoint() in python3.7 vs pdb in the earlier versions.

Breakpoints are generally the point in your code where you'd temporarily like to stop the execution of the program and do some value checks and look up the status of different objects in your program. This is done by hooking up a line just above the point where you'd like to debug.

## In the earlier versions of python, you'd do:
```python
def divide(divisor, dividend):
    import pdb; pdb.set_trace()
    return dividend / divisor

if __name__ == '__main__':
    print(divide(2, 0))
```

Running the above code in shell produces results as following:

```
$ python pdbexample.py
> /home/bhishan-1504/pdbexample.py(3)divide()
-> return dividend / divisor
(Pdb) args
divisor = 0
dividend = 4000
(Pdb) continue
Traceback (most recent call last):
  File "pdbexample.py", line 6, in 
    print(divide(0, 4000))
  File "pdbexample.py", line 3, in divide
    return dividend / divisor
ZeroDivisionError: integer division or modulo by zero
```

It enters an interactive mode, stopping the flow of program so you can strike commands to view the status of the program and continue or exit.

## Here is a list of few useful commands on the interactive mode:
| Command | Short form | What it does |
|---------|------------|--------------|
| args | a | Print the argument list of the current function |
| break | b | Creates a breakpoint (requires parameters) in the program execution |
| continue | c or cont | Continues program execution |
| help | h | Provides list of commands or help for a specified command |
| jump | j | Set the next line to be executed |
| list | l | Print the source code around the current line |
| next | n | Continue execution until the next line in the current function is reached or returns |
| step | s | Execute the current line, stopping at first possible occasion |
| pp | pp | Pretty-prints the value of the expression |
| quit or exit | q | Aborts the program |
| return | r | Continue execution until the current function returns |

## With python3.7 you'd do:
```python
def divide(divisor, dividend):
    breakpoint()
    return dividend / divisor

if __name__ == '__main__':
    print(divide(0, 4000))
```

```
$ python3.7 breakpointexample.py
> /home/bhishan-1504/pdbexample.py(3)divide()
-> return dividend / divisor
(Pdb) args
divisor = 0
dividend = 4000
(Pdb) continue
Traceback (most recent call last):
  File "pdbexample.py", line 6, in 
    print(divide(0, 4000))
  File "pdbexample.py", line 3, in divide
    return dividend / divisor
ZeroDivisionError: integer division or modulo by zero
```

Python3.7 comes with a built-in function named breakpoint() which enters the debugger at the call of site. While it is the same results, it is more intuitive and idiomatic.

## Why was this change necessary?
* In the earlier version, It's a lot to type. It also leads to typo.
* It ties debugging directly to the choice of pdb. There might be other debugging options, say if you're using an IDE or some other development environment.
* It is two statements import pdb and pdb.set_trace()
* This is also inspired from the JavaScript debugger statement js-debugger.

## More implementation details (From PEP 553):
Also with the new built-in breakpoint(), there are two new name bindings for the sys module, called sys.breakpointhook() and sys.__breakpointhook__. By default, sys.breakpointhook() implements the actual importing and entry into pdb.set_trace(), and it can be set to a different function to change the debugger that breakpoint() enters. This means there is no necessary ties to pdb in python3.7, you could use debugger of your choice.
sys.__breakpointhook__ is initialized to the same function as sys.breakpointhook() so that you can always easily reset sys.breakpointhook() to the default value (e.g. by doing sys.breakpointhook = sys.__breakpointhook__). The signature of the built-in is breakpoint(*args, **kws). The positional and keyword arguments are passed straight through to sys.breakpointhook() and the signatures must match or a TypeError will be raised. The return from sys.breakpointhook() is passed back up to, and returned from breakpoint().

Since with this new directive, you are not bound to only use the pdb but any other debugger, hence the positional (* args) argument and keyword (** kwargs) argument for the built-in breakpoint(* args, ** kwargs) makes sense. Unlike pdb other debugger might expect arguments.

The breakpointhook() default implementation consults environment variable named PYTHONBREAKPOINT for various behavior of the debugger.

The environment variable can have various values and hence the behavior of the debugger.

* PYTHONBREAKPOINT=0 disables debugging. Specifically, with this value sys.breakpointhook() returns None immediately.
* PYTHONBREAKPOINT= (i.e. the empty string). This is the same as not setting the environment variable at all, in which case pdb.set_trace() is run as usual.
* PYTHONBREAKPOINT=some.importable.callable. In this case, sys.breakpointhook() imports the some.importable module and gets the callable object from the resulting module, which it then calls.

This environment variable allows external processes to control how breakpoints are handled. Some uses cases include:

Completely disabling all accidental breakpoint() calls pushed to production. This could be accomplished by setting PYTHONBREAKPOINT=0 in the execution environment.

## Disabling debugging:
```
$ PYTHONBREAKPOINT=0 python3.7 breakpointexample.py
```
This will disable any breakpoint() calls in the program file.

## Run custom function on breakpoints:
With python3.7, what you could also do is execute a custom program/function where there is entry of breakpoint() in the program. One example where this is handy is when you want to get all the local variable's values on the current function before executing the following statements.

Let us define a custom function that we want being called at breakpoint:

```python
import sys
def local_variables():
    active = sys._getframe(1)
    print(active.f_locals)
```

```
$ PYTHONBREAKPOINT=custom_code.local_variables python3.7 breakpointexample.py
{'divisor': 0, 'dividend': 4000}
Traceback (most recent call last):
  File "pdbexample.py", line 6, in 
    print(divide(0, 4000))
  File "pdbexample.py", line 3, in divide
    return dividend / divisor
ZeroDivisionError: division by zero
```

That's my little introduction to the new built-in breakpoint() in Python3.7 . You should also read about Python Assignment Expression which has been accepted for Python3.8 http://www.thetaranights.com/python-assignment-expression-pep-572-python3-8/