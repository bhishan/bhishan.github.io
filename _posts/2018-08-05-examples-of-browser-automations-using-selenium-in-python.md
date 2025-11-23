---
title: 'Examples of Browser Automation using Selenium in Python'
date: 2018-08-05
permalink: /posts/2018/08/examples-of-browser-automation-using-selenium-in-python/
tags:
  - Python
  - Selenium
  - Browser Automation
  - Web Scraping
---

Browser Automation is one of the coolest things to do especially when there is a major purpose to it. Through this post, I intend to host a set of examples on browser automation using selenium in Python so people can take ideas from the code snippets below to perform browser automation as per their need. Selenium allows just about any kinds of interactions with the browser elements and hence is a go for tasks requiring user interaction and javascript support.

## Installation:

```
pip install selenium
Download chromedriver from http://chromedriver.chromium.org/downloads
Download phantomjs from http://phantomjs.org/download.html
```

## Login to a website using selenium

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

## Switching proxy with selenium

As much as selenium is used for web scraping, it is very effective for web interactions too. Suppose a scenario where you have to cast a vote for a competition, one vote per IP address. Following example demonstrates how you would use selenium to perform a repetitive task(casting a vote in this case) from various IP addresses.

```python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
url = "somedummysite.com/voting/bhishan.php" # url not made public



def cast_vote(proxy):
    service_args = [
    '--proxy=' + proxy,
    '--proxy-type=http',
    ]
    print(service_args)
    browser = webdriver.PhantomJS(service_args=service_args)
    
    browser.get(each_url)
    try:
        cast_vote_element = WebDriverWait(browser, 10).until(
            EC.presence_of_element_located((By.CLASS_NAME, 'vote'))
        )
    except selenium.common.exceptions.TimeoutException:
        print("Cast vote button not available. Seems like you have used this IP already!")
        return
    cast_vote_element.click()
    browser.quit()

def main():
    with open(proxies.txt', 'rb') as f:
        for each_ip in f:
            cast_vote(each_ip.strip())



if __name__ == '__main__':
    main()
```

## Execute JavaScript using selenium

There could be cases where you'd want to execute javascript on the browser instance. The below example is a depiction of one such scenario. Remember when in your News Feed on facebook, a post has hundreds of thousands of comments and you have to monotonously click to expand the comment threads. The example below does it through selenium but has an even bigger purpose. The following code snippet loops over a few thousand facebook urls(relating to a post) and expands the comment threads and prints the page as a pdf file. This was a part of a larger program that had something to do with the pdf files. However, it isn't relevant to this post. Here is a link to the JavaScript code which is used in the program below that expands the comments on facebook posts. I don't even remember where I found it though.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

import json
import time


# get the js to be executed.

with open('js_code.txt', 'r') as f:
    js_code = f.read()

executable_path = '/home/bhishan-1504/Downloads/chromedriver_linux64/chromedriver'


appState = {
    "recentDestinations": [
        {
            "id": "Save as PDF",
            "origin": "local"
        }
    ],
    "selectedDestinationId": "Save as PDF",
    "version": 2
}


profile = {"printing.print_preview_sticky_settings.appState": json.dumps(appState), 'savefile.default_directory': "/home/bhishan-1504/secret_project/"}

profile["download.prompt_for_download"] = False
profile["profile.default_content_setting_values.notifications"] = 2
chrome_options = webdriver.ChromeOptions()

chrome_options.add_experimental_option('prefs', profile)
chrome_options.add_argument("--start-maximized")
chrome_options.add_argument('--kiosk-printing')

# chrome_options.add_argument("download.default_directory=/home/bhishan-1504/secret_project/")
browser = webdriver.Chrome(executable_path=executable_path, chrome_options=chrome_options)

def save_pdf(count):
    browser.execute_script("document.title=" + str(count) + ";")
    browser.execute_script('window.print();')
    time.sleep(1)


def visit_page(url, count):
    browser.get(url)
    try:
        home_btn = WebDriverWait(browser, 10).until(
            EC.presence_of_element_located((By.LINK_TEXT, "Home"))
        )
    except selenium.common.exceptions.TimeoutException:
        print("Didn't work out!")
        return

    browser.execute_script(js_code)
    time.sleep(7)
    save_pdf(count)




if __name__ == '__main__':
    count = 1
    # loop through the text file and pass to visit page function.
    with open('urls.txt', 'r') as f:
        for each_url in f.readlines():
            visit_page(each_url, count)
            count += 1
```

I recently published an article on Web Scraping using BeautifulSoup. You should read it.