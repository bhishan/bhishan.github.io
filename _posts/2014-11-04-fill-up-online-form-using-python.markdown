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

**Value of name attribute for each text entry box**

On each text entry box, right click and inspect elements and look for the value of name attribute.

In case of github ::
--------------------
1. For username field
{% highlight python %}
<input type="text" name="user[login]" class="textfield" placeholder="Pick a username" data-autocheck-url="/signup_check/username" autofocus="">
{% endhighlight %}
The above code is the result of inspect element over the **username** text entry area. All we want is the value assigned to the name attribute. In this case it is **"user[login]"**.

Similarly for the **email address** text field, the value of the name attribute is **"user[email]"**.
And for the **password** entry field, the value of the name attribute is **"user[password]"**.

Thats all the information we need. The below code should sign up for a git account. 
{% highlight python %}
import mechanize  #sudo pip install python-mechanize
br = mechanize.Browser()  #initiating a browser
br.set_handle_robots(False)  #ignore robots.txt
br.addheaders = [("User-agent","Mozilla/5.0")]  #our identity 
gitbot = br.open("https://github.com")  #requesting the github base url
br.select_form(nr=2)  #the sign up form in github is in third position(search and sign in forms come before signup)
br["user[login]"] = username  #username for github
br["user[email]"] = email  #email for github
br["user[password]"] = password  #password for github
sign_up = br.submit()
{% endhighlight %}
