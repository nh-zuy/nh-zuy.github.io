---
layout: post
title: A simple way to parse a website
date: 2020-01-17 09:49:09 +0700
categories: ['Technique']
tags: ['tips']
---

#### The first time I've parsed a website

	Early, I found it diffucult to parse a website because I didn't know how to do it right. After a while of searching GG and Youtube, I realized that bs4 module can help me.

* Conditions: Python3.x, a little understanding of HTML, working with file .csv, ... brain :v

OK, let's get started!

* First, import some libraries can help us with parsing our website:
{% highlight ruby %}
	from bs4 import BeautifulSoup
	from urllib.request import urlopen
	import csv
{% endhighlight %}

* Get the URL of your website you want to parse and make a request( Remember to close it :v):
{% highlight ruby %}
	url = "https://www.newegg.com/global/vn-en/p/pl?Submit=StoreIM&page=1&Depa=3&Category=223"
	page = urlopen(url)
	Soup = BeautifulSoup(page, "html.parser")
	page.close()
{% endhighlight %}

* Now, we're going to take the information of the laptops:
{% highlight ruby %}
	Laptops = Soup.findAll('div', class_="item-container").find('div', class_="item-info")
{% endhighlight %}

* Make a file .csv to store the info of each laptop:
{% highlight ruby %}
	with open('ngu.csv', mode='w') as file:
		writer = csv.writer(file)
		writer.writerow(['BRAND', 'INFO', 'PRICE', 'SUP_PRICE'])
		for laptop in Laptops:
			brand = laptop.a.img.get('title')
			info = laptop.find('a', class_="item-info").text

			laptop_action = laptop.find('div', class_="item-action").ul

			current_price = laptop_action.find('li', class_="price-current").strong.text
			sup_price = laptop_action.find('li', class_="price-current").sup.text
{% endhighlight %}

* Last, we're going to check the result in "ngu.csv" file ^^
			

















