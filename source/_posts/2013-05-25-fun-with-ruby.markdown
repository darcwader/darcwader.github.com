---
layout: post
title: "Fun with Ruby"
date: 2013-05-25 17:44
comments: true
categories: [ "Ruby" , "Programming" ]
---

I am lazy and hate using the mouse. So I decided to use ruby to pull my techcrunch news for me.

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

