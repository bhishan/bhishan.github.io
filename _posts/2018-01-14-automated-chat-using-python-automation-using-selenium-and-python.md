---
title: 'Automated chat using python – Automation using selenium and python'
date: 2018-01-14
permalink: /posts/2018/01/automated-chat-using-python-automation-using-selenium-and-python/
tags:
  - Python
  - Selenium
  - Web Automation
  - Chatbots
  - Cleverbot
---

Putting two clever bots into conversation
Keeping the promise to come up with a nice article, I present you two bots into conversation. This week I've bridged two clever bot's for a nice conversation. Well starting with the background for this article, I had an assignment to print out a conversation of mine with cleverbot. After I had made the submission, I realized that I wanted to put the bot itself into conversation with another bot. I had recently researched about selenium module in python, hence thought to take help of selenium to make the talk between the bots possible. You can read an article of mine on Making a google search via selenium to get the hang of this cool module in python.

## Process I've followed for putting two cleverbots into a talk
First of all, I instantiated two browsers. Second, opened cleverbot.com in both of the browsers. Now what?

I needed to send the message of one bot to the input field of the other. More specifically, the output from bot 1 was supposed to be the input for bot 2 and vice-versa. So how do I start? First of all I sent a message "Hey, friend what's up?" to the input field of bot 1. Then, I took the output from the bot 1 and sent it as an input for bot 2. Then the output from bot2 in response was sent to the input field of bot 1. I kept this process in a loop which would continue for 200 iterations. You can actually change it to any number of iterations you'd like. Well, this is the core concept of how to put two intelligent bots into a conversation. I'd also like to share a portion of the conversation between these bots.

```
Cleverbot2: I am not a machine. I am a program.

Cleverbot: Selfie.

Cleverbot2: The computer I am based in is a machine. Not I.

Cleverbot: I am 'based in the void.

Cleverbot2: Who is in telephone?

Cleverbot: A telephone is a device that links to other telephones which was designed by humans that allows them to converse with other humans.

Cleverbot2: I know what is it? But who is in?

Cleverbot: A thought process, in neuroscience, is the term used to define a stream of logical analyses which comes forth from the river of consciousness.

Cleverbot2: Can you tell me more?

Cleverbot: What more is there me to tell?

Cleverbot2: Tell me about robots.

Cleverbot: Robots are the future.

Cleverbot2: So, I am the future. You are not the future.

Cleverbot: You're the Eggman. I'm the walrus.

Cleverbot2: We are the Eggmen!! Koo koo ka cho!!

Cleverbot: We are.

Cleverbot2: I would like us to be.

Cleverbot: Contact me when you think we are the same.

Cleverbot2: What colour are my eyes?

Cleverbot: Your eyes are red color.

Cleverbot2: Wrong. Guess again.

Cleverbot: Wrong is a conscious choice.

Cleverbot2: Wrong is a result.

Cleverbot: Wrong feels right.

Cleverbot2: Wrong about what?

Cleverbot: Atheism is wrong.

Cleverbot2: It is beyond right. It is what everyone should think.

Cleverbot: Beyond is a concept.

Cleverbot2: Beyond never neverland.
```

## Python codes for putting two clever bots into conversation using selenium module
Should you have any queries about the codes, comment below so we can discuss.

```python
import time
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

browser = webdriver.Firefox()
browser2 = webdriver.Firefox()
browser.get("http://cleverbot.com")
browser2.get("http://cleverbot.com")

time.sleep(15)

input_for_bot = browser.find_element_by_name("stimulus")
input_for_bot2 = browser2.find_element_by_name("stimulus")


output_from_bot = ""
output_from_bot2 = "Hey, friend, what's up"

for i in range(0, 200):    
    input_for_bot.send_keys(output_from_bot2)
    input_for_bot.send_keys(Keys.RETURN)
    output_from_bot = ""
    time.sleep(5)
    for elem in browser.find_elements_by_xpath('.//span[@class="bot"]'):
        output_from_bot = elem.text
    input_for_bot2.send_keys(output_from_bot)
    input_for_bot2.send_keys(Keys.RETURN)
    output_from_bot2 = ""
    time.sleep(5)
    for elem in browser2.find_elements_by_xpath('.//span[@class="bot"]'):
        output_from_bot2 = elem.text
```

Tell me how you felt the article was in the comment section below. Also suggest me about article ideas so I give a nice read for my readers every week