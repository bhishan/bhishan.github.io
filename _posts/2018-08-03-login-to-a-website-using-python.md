---
title: 'Login to a website using python'
date: 2018-08-03
permalink: /posts/2018/08/login-to-a-website-using-python/
tags:
  - Python
  - Web Automation
  - Mechanize
  - MechanicalSoup
  - Selenium
---

Python is often used for web automation, scraping and process automation. Through this post, I intend to host a set of example code snippets to login to a website programmatically. Often the initial step of web scraping or web process automation is to login to the source website. There are various modules in Python that abstract the process behind the login and allow us for a clean set of methods to interact with websites and web elements. We will be using a few of them in this post.

## Login to website using mechanize

**Installation(Python2)**
```
pip install mechanize
```

```python
>>> browser = mechanize.Browser()
>>> browser.set_handle_robots(False)
>>> browser.addheaders = [("User-agent","Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.2.13) Gecko/20101206 Ubuntu/10.10 (maverick) Firefox/3.6.13")]
>>> browser.open("https://github.com/login")
>>> for each in browser.forms():
...     print(each)
...
<POST https://github.com/site/dismiss_unsupported_browser application/x-www-form-urlencoded
  <HiddenControl(utf8=✓) (readonly)>
  <HiddenControl(authenticity_token=WY8fq1ymH5h3W6kLY2XebhbUrxp/1l1Az8AUIq0UvG3PDzsPYIllp8aL8muHv+RIBeFM70hxVJt8Ek+JKlLMFQ==) (readonly)>
  <SubmitButtonControl(<None>=) (readonly)>>
<POST https://github.com/session application/x-www-form-urlencoded
  <HiddenControl(utf8=✓) (readonly)>
  <HiddenControl(authenticity_token=DnEB5urKBrijeErqx/JIz4fV6s074zU//4CYZUNRUUHq2TuEMQvjLQTH52vC2SL1EzquXIaeFFGk+UZpbv9uDA==) (readonly)>
  <TextControl(login=)>
  <PasswordControl(password=)>
  <SubmitControl(commit=Sign in) (readonly)>>

>>> browser.select_form(nr=1) # selecting the second form i.e the login form. Form index starts from 0.
>>> browser['login'] = 'bhishan'
>>> browser['password'] = 'password'
>>> response_login = browser.submit()
```

## Login to website using mechanicalsoup(mechanize alternative for python3)

**Installations:**
```
pip install MechanicalSoup
```

Python3 doesn't have support for mechanize. MechanicalSoup can be thought of as a combination of mechanize and BeautifulSoup. If you are interested into webscraping with BeautifulSoup, here is a post that might be helpful. https://www.thetaranights.com/web-scraping-beautifulsoup-python/

```python
>>> import mechanicalsoup
>>> browser = mechanicalsoup.Browser()
>>> login_page = browser.get("https://github.com/login")
>>> login_form = login_page.soup.find("form")
>>> login_form
<form accept-charset="UTF-8" action="/session" method="post"><input name="utf8" type="hidden" value="✓"/><input name="authenticity_token" type="hidden" value="zB5/vf7oMWaGBehYvZlW5sD9QNdsMV7J1YZV82jpo5/q/5woif7PCvTeg/lHvrtCGkwTfq3s2+Vli7ZaCnKmlA=="/> <div class="auth-form-header p-0">
<h1>Sign in to GitHub</h1>
</div>
<div id="js-flash-container">
</div>
<div class="auth-form-body mt-3">
<label for="login_field">
          Username or email address
        </label>
<input autocapitalize="off" autocorrect="off" autofocus="autofocus" class="form-control input-block" id="login_field" name="login" tabindex="1" type="text"/>
<label for="password">
          Password <a class="label-link" href="/password_reset">Forgot password?</a>
</label>
<input class="form-control form-control input-block" id="password" name="password" tabindex="2" type="password"/>
<input class="btn btn-primary btn-block" data-disable-with="Signing in…" name="commit" tabindex="3" type="submit" value="Sign in"/>
</div>
</form>

>>> login_form.find("input", {"name": "login"})["value"] = "bhishan"
>>> login_form.find("input", {"name": "password"})["value"] = "password"
>>> login_response = browser.submit(login_form, login_page.url)
```

## Login to a website using selenium python

Now, when you need to interact with the web elements, execute javascript, print pdfs, etc you would use selenium.

**Installations:**

```
pip install selenium
Download chromedriver from http://chromedriver.chromium.org/downloads
```

```python
>>> from selenium import webdriver
>>> from selenium.webdriver.common.keys import Keys
>>> executable_path = "/home/bhishan-1504/Downloads/chromedriver_linux64/chromedriver"
>>> browser = webdriver.Chrome(executable_path=executable_path)
>>> browser.get("https://github.com/login")
>>> username_field = browser.find_element_by_name("login")
>>> password_field = browser.find_element_by_name("password")
>>> username_field.send_keys("bhishan")
>>> password_field.send_keys("password")
>>> password_field.send_keys(Keys.RETURN)
>>>
```