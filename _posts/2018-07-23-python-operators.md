---
title: 'Python Operators'
date: 2018-07-23
permalink: /posts/2018/07/python-operators/
tags:
  - Python
  - Operators
  - Programming
  - Fundamentals
---

Operators are the constructs that enable performing operations on operands(values and variables). The operators in python are represented by special symbols and keywords. The intentions of this blog is to familiarize with the various operators in Python.

## Arithmetic Operators
These operators are used to perform mathematical operations ranging from addition, subtraction, multiplication, division to modulus, exponent, etc. Following table shows the arithmetic operators and it's usage:

| Operator | Usage | Description |
|----------|-------|-------------|
| + | a + b | Add values on either side of the operator or unary plus |
| - | a -b | Subtract right hand operand from the left hand operand. Also unary negation |
| * | a * b | Multiply values on either side of the operator |
| / | a / b | Divides left hand operand by right hand operand |
| % | a % b | Returns the remainder from dividing left hand operand by right hand operand |
| ** | a ** b | Returns Exponent – left operand raised to the power of right |
| // | a //b | Floor Division – The division of operands where the result is the quotient in which the digits after the decimal point are removed. But if one of the operands is negative, the result is floored, i.e., rounded away from zero (towards negative infinity) − |

## Comparison Operators
The comparison operators are used to identify relation between operands on either side of the operator. These are also called relational operators. The values from the comparison operators is either True or False.

| Operator | Description | Usage |
|----------|-------------|-------|
| == | Returns True if the values on the either side of the operator is equal otherwise False. | a == b |
| != | Returns True if the values on either sides of the operator is not equal to each other otherwise False. | a != b |
| > | Returns True if the value of the operand on the left of the operator is greater than the value on the right side of the operator. | a >b |
| < | Returns True if the value of the operand on the left of the operator is less than the value on the right side of the operator. | a < b |
| >= | Returns True if the value of the operand on the left of the operator is greater than or equal to the value on the right side of the operator. | a >= b |
| <= | Returns True if the value of the operand on the left of the operator is less than or equal to the value on the right side of the operator. | a <= b |

## Assignment Operators
Assignment operators are used for assigning the value from the right operand of the operator to the left operand. Following is the various assignment operators in Python:

| Operator | Description | Usage | Equivalent to |
|----------|-------------|-------|---------------|
| = | Assigns values from right side operands to left side operand | c = a + b | c = a + b |
| += | Adds the value of right operand to the value of left operand and assign the result to the left operand | b += a | b = b + a |
| -= | Subtracts the value of right operand from the value of left operand and assign the result to left operand | b -= a | b = b – a |
| *= | Multiplies the value of right operand with the value of the left operand and assigns the result to left operand | b *= a | b = b * a |
| /= | Divides the value of the left operand with the value of the right operand and assigns the result to the left operand | b /= a | b = b / a |
| %= | Assigns the remainder from dividing left hand operand by right hand operand to the left hand operand | b %= a | b = b % a |
| **= | Assigns the value from the exponential operation to the left operand. | b **= a | b = b ** a |
| //= | Performs floor division on operators and assign value to the left operand | b //= a is equivalent to | b = b // a |

## Python Logical Operators
Logical operators in python are used for conditional statements which evaluates to either true or false. AND, OR, NOT are the logical operators in python.

| Operator | Description | Usage |
|----------|-------------|-------|
| and | True if both sides of the operator is True | x and y |
| or | True if either of the operand is True | x or y |
| not | Complements the operand | not val |

## Membership Operator
These operators test for membership(presence) in a sequence such as string, list or tuple. Following are the membership operators:

| Operator | Description | Usage |
|----------|-------------|-------|
| in | True if the value/operand in the left of the operator is present in the sequence in the right of the operator. | x in y |
| not in | True if the value/operand in the left of the operator is not present in the sequence in the right of the operator. | x not in y |

## Identity Operator
It is used to compare the memory location of two python objects .i.e both the operands refer to the same object.

| Operator | Description | Usage |
|----------|-------------|-------|
| is | True if both the operands refer to the same object. | x is True |
| is not | Evaluates to false if the variables on either side of the operator point to the same object and true otherwise. | x is not True |

## Bitwise Operators
Bitwise operators work on bits of an operand hence the name.

```python
>>> # Bitwise AND
...
>>> a = 3
>>> b = 4
>>> a = 3 # equivalent binary is 0011
>>> b = 4 # equivalent binary is 0100
>>> a & b
0
>>>
>>> # Bitwise OR
...
>>> a | b
7
>>> # 7 is equivalent to 0111 in binary
...
>>>
>>> # Bitwise NOT
...
>>> ~ a
-4
>>>
>>> # Bitwise XOR
...
>>> a ^ b
7
>>>
>>> # Bitwise right shift
...
>>> a >> 2
0
>>>
>>> # Bitwise left shift
...
>>> a << 2
12
```