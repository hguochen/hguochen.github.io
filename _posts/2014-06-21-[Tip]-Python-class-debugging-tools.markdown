---
title: "[Tip] Python class debugging tools"
layout: post
date: 2014-06-21 17:59:42
comments: true
categories: [programming, python]
---

It can be tough trying to debug classes, inherited classes, class methods etc in general. Here's 2 useful debugging utility functions i love to use to sort through the mess.

### 1. Printing attributes and values

	def print_attr(obj):
		"""
		Traverses the items in the object's dictionary and prints each attribute name and its corresponding value.
		"""
		for attr in obj.__dict__:
			print attr, getattr(obj, attr)

As explained in docstring, function print_attr() takes in an object and spits out each of its attributes and its corresponding value.

The built-in function getattr() takes an object and an attribute name(as string) and returns the attribute's value.

### 2. Find defining class

	def find_defining_class(obj, method_name):
	"""
	Uses method resolution method(MRO) to get the list of class objects(types) that will be searched for methods.
	Return the class to which the method 'method_name' belong to.
	"""
	for origin in type(obj).mro():
		if method_name in origin.__dict__:
			return origin

Useful if your class belongs to a long chain of super classes and you are trying to find out which class does a particular method belong to. This method works because of [Method Resolution Order](https://www.python.org/download/releases/2.3/mro/) rule in python.

Here's an example:
	
	>>> hand = Hand()
	>>> print find_defining_class(hand, 'shuffle)
	<class 'Card.Deck'>

So the shuffle method for this Hang object is the one in Deck.

Hope this 2 utility debugging functions can save you time traversing those pesky bugs. Please share your secret debugging functions too! 

Happy debugging! Ok fine, that's a fallacy...Don't die debugging!