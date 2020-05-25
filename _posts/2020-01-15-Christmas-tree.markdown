---
layout: post
title: How to build a Christmas Tree
date: 2020-01-15 09:16:45 +0700
categories: ['Python']
tags: ['python']
---

#### MAKE A CHRISTMAS TREE IN PYTHON

1.**IDEA**: We will use a main loop that looping the tree with some colored-dots after a few seconnds. After each loop, the colored-dots get on/off such as: red->none, blue->none, ..., by using the Threading module.

1.**Implement**:

* Take a Christmas tree picture in ASCII code, put it into file "ChristmasTree.txt". Changing some characters into R, G, Y or B( equivalent to red, green, yellow and blue).

* Import some module necessary 
{% highlight ruby %}
	import os
	import threading
	import time
{% endhighlight %}

* Reading the file into a list:
{% highlight ruby %}
	with open('ChristmasTree.txt', 'r') as file:
		Tree = list(file.read().rstrip())
{% endhighlight %}

* Looping the list and save the index of the colored-dots:
{% highlight ruby %}
	Red = []
	Green = []
	Yellow = []
	Blue = []

	for index, character in enumerate(Tree):
		if character == 'R':
			Red.append(index);
			Tree[index] = O;
		elif character == 'Y':
			Yellow.append(index);
			Tree[index] = O;
		elif character == 'G':
			Green.append(index);
			Tree[index] = O;
		elif character == 'B':
			Blue.append(index);
			Tree[index] = O;
{% endhighlight %}

* We continue to build 2 functions for change the colour of the colored-dots and looping the on/off:
{% highlight ruby %}
	def Colored_form(color):
		if color == 'Red':
			return "\033[91mO\033[0m"
		elif color == 'Green':
			return "\033[92mO\033[0m"
		elif color == 'Yellow':
			return "\033[93mO\033[0m"
		elif color == 'Blue':
			return "\033[94mO\033[0m"

	def Transform(color, indexes):
		off = False
		while True:
			for index in indexes:
				Tree[index] = ColoredForm(color) if not off else 'O'
			os.system('cls' if __name__ == 'nt' else 'clear')
			print(''.join(Tree))

			off = not off
			time.sleep(1.5)
{% endhighlight %}

* Now, we have the main function to loop the tree, we continue to set up 4 thread of Red, Green, Yellow, Blue, run in parallel:
{% highlight ruby %}
	Red_thread = threading.Thread(target=Transform, argus=('Red', Red))
	Green_thread = threading.Thread(target=Transform, argus=('Green', Green))
	Yellow_thread= threading.Thread(target=Transform, argus=('Yellow', Yellow))
	Blue_thread = threading.Thread(target=Transform, argus=('Blue', Blue))

	for thread in [Red_thread, Green_thread, Yellow_thread, Blue_thread]:
		thread.start()
	
	for thread in [Red_thread, Green_thread, Yellow_thread, Blue_thread]:
		thread.join()
{% endhighlight %}

* Take a lock and release it when draw new screen:
{% highlight ruby %}
	Lock = threading.Lock()
	...
	Lock.acquire()
	Lock.release()
{% endhighlight %}

* Last, full of program:
{% highlight ruby %}
	#!/usr/bin/env python

	import os
	import time
	import threading


	with open('ChristmasTree.txt', 'r') as file:
		Tree = list(file.read().rstrip())

	Lock = threading.Lock()

	def Colored_form(color):
		if color == 'Red':
			return "\033[91mO\033[0m"
		elif color == 'Green':
			return "\033[92mO\033[0m"
		elif color == 'Yellow':
			return "\033[93mO\033[0m"
		elif color == 'Blue':
			return "\033[94mO\033[0m"

	def Transform(color, indexes):
		off = False
		while True:
			for index in indexes:
				Tree[index] = Colored_form(color) if not off else 'O'

			Lock.acquire()
			os.system('cls' if __name__=='nt' else 'clear')
			print(''.join(Tree))
			Lock.release()

			off = not off
			time.sleep(1.5)

	Red = []
	Green = []
	Yellow = []
	Blue = []

	for index, character in enumerate(Tree):
		if character == 'R':
			Red.append(index)
			Tree[index] = 0
		elif character == 'Y':
			Yellow.append(index)
			Tree[index] = 0
		elif character == 'G':
			Green.append(index)
			Tree[index] = 0
		elif character == 'B':
			Blue.append(index)
			Tree[index] = 0

Red_thread = threading.Thread(target = Transform, args = ('Red', Red))
Green_thread = threading.Thread(target = Transform, args = ('Green', Green))
Yellow_thread = threading.Thread(target = Transform, args = ('Yellow', Yellow))
Blue_thread = threading.Thread(target = Transform, args = ('Blue', Blue))

for thread in [Red_thread, Green_thread, Yellow_thread, Blue_thread]:
	thread.start()
for thread in [Red_thread, Green_thread, Yellow_thread, Blue_thread]:
	thread.join()
{% endhighlight %}

	






	













