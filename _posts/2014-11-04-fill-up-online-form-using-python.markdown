---
layout: post
title: "Fill up an online form using python"
date: 2014-11-04
---

By the end of this read, you will be able to fill up an online form using python. For this thing to be done, I would like to introduce you to a module ***"mechanize"***.
Mechanize is essentially designed for this purpose. One advantage while working with mechanize is- we need not worry about things like session handling. 

Let's start with github signup form itself.

**Base url of the github page**
https://www.github.com/

**Index of the form to fill-up**
Index of a form is the count of forms that exists before it. A webpage may contain several forms. If the form you want to work with is present after two other forms while viewing the source code then the form index would be 2 (2 forms are before it). This is the case for sign up form in github.
{% highlight python %}
def print_hi(name)
  print name
print_hi('Tom')
{% endhighlight %}
