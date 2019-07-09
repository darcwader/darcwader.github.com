---
title: "Fun with Ruby"
date: 2013-05-25T17:44:04+05:30
comments: true
categories: [ "Ruby" , "Programming" ]
---

I am lazy and hate using the mouse. So I decided to use ruby to pull my techcrunch news for me.
I just started learning ruby so don't start sending me hate mails if this is too lame for you. 

## Aim

screenscrape a webpage and extract some information

## Gems

First we need a way to fetch the webpage. the best one i found was 'Rest-Client'. it has a very easy interface to get any webpage

```
gem install 'rest-client'

irb
require 'rest-client'
response = RestClient.get "http://techcrunch.com"
```

Once I got the response, I need a way to parse the response which is html. 'Nokogiri' was a good one.

Nokogiri documentation gives this example.
```
doc = Nokogiri::HTML(open('http://www.google.com/search?q=sparklemotion'))

doc.css('h3.r a').each do |link|
puts link.content
end
```

### Script

so lets get started

* Get the techcruch page
* Process to get all headlines 
* Print the headlines

```
require 'rest-client'
require 'Nokogiri'

response = RestClient.get "http://techcrunch.com"
html = Nokogiri::HTML(response)

html.css("h2.headline").each do |headline|
  puts headline
end
```

If you say, why not just use RSS? I Would say the title of this article is 'Fun with ruby' not 'Dummmies guide to RSS'.

## Watir

All is good. but what if I have to login and then start to extract information. It might get tricky.

Then comes (Watir)[http://watir.com/] it is an amazing automation tool. you can launch a browser and control it through ruby, it is based on selenium. 

The steps now would be

* Launch Watir browser and login
* Start scraping the information


```
require 'watir-webdriver'
browser = Watir::Browsser.new

browser.goto "http://techcrunch.com"
html = browser.html

```

use this html to parse same way as before.

## Screenshot

If you would want to take a screenshot you can 

```
browser.screenshot.save "screenshot.png"
```

## Whats the use?

Well , If you are irritated with JIRA maybe you can write a program to log your hours easily.
Maybe you want to spam someones facebook wall.
Send gazillion mails to your ex.
Try to download entire internet onto your maching.

go figure!



