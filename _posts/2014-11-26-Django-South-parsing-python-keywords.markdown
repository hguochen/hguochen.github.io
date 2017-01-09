---
title: "Django South incorrectly parsing python keywords"
layout: post
date: 2014-11-26 09:48:29
comments: true
categories: [web development]
---


Code base below is an expert from a production level [Django](https://www.djangoproject.com) stack using [Django South 0.8.4](https://south.readthedocs.org/en/latest/) as a database migration tool.



Came across a bloc of code on Django Models looking like this earlier:

![django_models](http://i1218.photobucket.com/albums/dd420/hguochen/Screenshot2014-11-26095115.png)

The category class inherits the django model class and extends a field called 'type' as a varchar field in SQL database.

See the problem? The variable **type** is a python keyword and should never be declared as a variable field in a python module. I would expect a SyntaxError be thrown when South parses the `models.py` file for migration but it does not. As it turns out, the migration file is actually parsed correctly as 'intended'.

![migration_file](http://i1218.photobucket.com/albums/dd420/hguochen/Screenshot2014-11-26102117.png)

I went to dig up the south documentation API that handles this:

![south_doc_api](http://i1218.photobucket.com/albums/dd420/hguochen/Screenshot2014-11-26102443.png =x350)

As seen, South simply parses the field_name as a string without performing any form of python keyword checking on the input. This is not right and should be changed. Not only does it confuse the developer but could potentially corrupt the database. With the current implementation, we could potentially have field names like '_-_---____', '%*@#^' or even ':)'. Imagine the kind of SQL statments you have to write to retrieve those fields :D.

Until South do something about this, we should consciously avoid using python keywords even if it works for the migration. As a quick hack, run

	python <filename>.py
	
to quickly check for any SyntaxErrors in the file before further actions.