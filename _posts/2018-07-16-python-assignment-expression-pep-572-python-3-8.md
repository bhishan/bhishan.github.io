---
title: 'Python Assignment Expression (PEP 572) - Python 3.8'
date: 2018-07-16
permalink: /posts/2018/07/python-assignment-expression-pep-572-python-3-8/
tags:
  - Python
  - PEP 572
  - Python 3.8
  - Assignment Expression
---

A recent buzz in the Python Community is PEP 572's acceptance for Python3.8 .
PEP stands for Python Enhancement Proposals and each such PEPs are assigned a number by the PEP editors and once assigned are never changed.

## What exactly is PEP 572(Directly from PEP 572)?
**Abstract**
This is a proposal for creating a way to assign to variables within an expression using the notation NAME := expr. A new exception, TargetScopeError is added, and there is one change to evaluation order.

**Rationale**
Naming the result of an expression is an important part of programming, allowing a descriptive name to be used in place of a longer expression, and permitting reuse. Currently, this feature is available only in statement form, making it unavailable in list comprehensions and other expression contexts.
Additionally, naming sub-parts of a large expression can assist an interactive debugger, providing useful display hooks and partial results. Without a way to capture sub-expressions inline, this would require refactoring of the original code; with assignment expressions, this merely requires the insertion of a few name := markers. Removing the need to refactor reduces the likelihood that the code be inadvertently changed as part of debugging (a common cause of Heisenbugs), and is easier to dictate to another programmer.

## What are assignment expressions?
As of now, in Python, assignment has to be a statement. This restricts for an example assignments from within if or while statements. Therefore following would be a Syntax Error in Python:

```python
if x = foo():
    # do something
else:
    # do something else
```

If this were made valid in python, it would have led to errors for confusing an assignment (=) with comparison operator (==). The code would then still execute without errors but produce unintended results.

Interestingly PEP 572 introduces a new operator := that assigns and returns a value. This is no replacement for the assignment operator and has a different purpose. Let us see the use case:

In most contexts where arbitrary Python expressions can be used, a named expression can appear. This is of the form NAME := expr where expr is any valid Python expression other than an unparenthesized tuple, and NAME is an identifier.
The value of such a named expression is the same as the incorporated expression, with the additional side-effect that the target is assigned that value:

**Our scenario: We want to process the contents of a file in a chunk-wise fashion. What we would naturally do is :**

```python
chunk = file.read(64)
while chunk:
    process(chunk)
    chunk = file.read(64)
```

The above code has redundancy, the need to do file.read(64) twice.

**You could also do the following to avoid redundancy:**

```python
while True:
    chunk = file.read(64)
    if not chunk:
        break
    process(chunk)
```

The above doesn't communicate the intent very well.

**With assignment expression, you could do:**

```python
while chunk := file.read(64):
   process(chunk)
```

Remember, we discussed := being assignment and return. The assignment to chunk happens at the while expression which also makes the data locally available in addition to deciding whether or not to exit the loop. It has no redundancy and communicates the intent very gracefully. That's pretty awesome or is it not? There has been a lot of discussions regarding this on the internet.

**One other example for use case of assignment expression:**

```python
# Share a subexpression between a comprehension filter clause and its output
filtered_data = [y for x in data if (y := f(x)) is not None]
```

PEP 572 has been discussed greatly on various forums such as reddit, hackernews. Here are a few such threads that are actually interesting to go through and bird a variety of viewpoints and opinions.
https://www.reddit.com/r/Python/comments/8ylelb/feedback_on_draft_post_to_pythonideas/
https://news.ycombinator.com/item?id=17448439