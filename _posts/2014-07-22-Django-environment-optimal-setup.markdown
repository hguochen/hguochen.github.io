---
title: "Django environment optimal setup"
layout: post
date: 2014-07-22 17:08:54
comments: true
categories: [web development]
---

[Django](https://www.djangoproject.com) is a great web framework based on MVC architecture that allows you to quickly and correctly build and deploy web projects. However, staring a new Django project can be a little daunting and often the wrong setup can lead to long term problems for your project. Here i'm sharing with you my preferred workflow on starting a new Django project. A small bit of planning goes a long way towards making life easier!

### Prerequisites
A working knowledge of python language and Django framework is assumed. You will also have to know [git](http://git-scm.com) for version control.

### Install Python
Head over to [python.org](http://python.org) and find the install instructions for your specific OS. For my development environment, i'm running MAC OSX Mavericks.

To check you have installed python correctly, type

	$ python
	
into terminal command line and you should get the following output:

	Python 2.7.5 (default, Mar  9 2014, 22:15:05) 
	[GCC 4.2.1 Compatible Apple LLVM 5.0 (clang-500.0.68)] on darwin
	Type "help", "copyright", "credits" or "license" for more information.
	>>>

### Install pip

[Pip](http://pip.readthedocs.org/en/latest/index.html) is a great tool for installing and managing python packages. We are going to use pip to install all third party packages to supplement our project. 

The easy way to install pip is to first download [get-pip.py](https://bootstrap.pypa.io/get-pip.py). Then run the following command in the folder with the downloaded file on your terminal:

	$ python get-pip.py
	
And you are done installing! You can test pip installed correctly with the command:

	pip -V

which shows the version of your pip.

### Install virtualenv
[Virutalenv](http://virtualenv.readthedocs.org/en/latest/) is a great tool to sandbox your development environments so you don't have to switch back and forth between different versions of softwares. What's more, the excellent extension [virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/) makes using virtualenv alot easier. This is common, recommend practice among python, Django users alike.

We are also going to use [pip](https://pypi.python.org/pypi) to install packages.

First, let's install virtualenv. Note that you can get both virtualenv and virtualenvwrapp install just by install ing virtualenvwrapper.

	$ pip install virtualenvwrapper

After it's installed, add following lines to your .bashrc file:

	export WORKON_HOME=$HOME/.virtualenvs
	export PROJECT_HOME=$HOME/directory-you-do-development-in
	source /usr/local/bin/virtualenvwrapper.sh

Then type the following command to 'activate' your modified .bashrc file:

	$ source .bashrc
	
### Creating new environment

To create a new virtual environment, type:

	$ mkvirtualenv django_project

where django_project is whatever name your project is. You should see your shell is now prepended by (django_project).

The django_project prepending is a reminder that you are using virtualenv instal of your system's Python installation. To exit, the virtual environment, simple type deactivate. When you want to resume work on your project, it's as easy as typing

	$ workon django_project

Where you run the command doesn't matter.

### Install Django

Since we are working in virtualenv, we have to install Django into this virtual environment. This makes sure that the version of Django we use for this project is not confused with other versions used by the system or other virtual environments.

	$ pip install django
	
This should give you the latest version of Django which will be installed in your virtualenv area. You can confirm this by doing:

	$ which django-admin.py
	
Which should point to your $HOME/.virtualenvs/django_project/bin/django-admin.py.

### Setup project folder
Now we can create our project directory. I highly recommend using [two scoops Django](https://github.com/twoscoops/django-twoscoops-project) project template for all Django projects. If you are new to two scoops django, it is actually a compilation of some of the best practices for developing Django projects. You can purchase a copy of this amazing book [here](http://twoscoopspress.org/).

Now go ahead and setup the project with this command:

	$ django-admin.py startproject --template=https://github.com/twoscoops/django-twoscoops-project/archive/master.zip --extension=py,rst,html django_project
	
Note that this command creates a django_project directory with the two scoops django template. I'll leave you to read more about [two scoops django template](https://github.com/twoscoops/django-twoscoops-project) yourself.

### Source control

Git is automatically installed in your virtual environment when you created it with virtualenvwrapper earlier.

Since our folder and file structure is up now, let's commit them to source control right from the start. Here's how to do that in git:

	$ git init
	
Creates a git repository in the current directory. Now let's stage all of our files to git to be commited.

	$ git add django_project
	
Now we commit them to our new repository:

	$ git commit -m "Initial commit of django_project"
	
Let's add the remote repository in [github](http://github.com):

	$ git remote add origin <remote repository URL>
	
Finally, let's push our changes!

	$ git push origin master
	
### South for database migrations

South is a great tool that detects changes to your models layer and automatically sync the changes to your database without you having to dabble with the database itself. This is done through a migration file that South creates. This lets you both migrate the database forward for new change and backward to undo a change or series of changes. 

In our virtualenv, install south using pip:

	$ pip install south
	
Then setup south with the project by adding it to our INSTALLED_APPS in settings/base.py file for the project. 
After which, run the command:

	# python manage.py syncdb

to sync the changes and south will be registered along with the project. South will also automatically setup the database with the tables it needs. 

Before we commit, we will want tot rack our install Python packages. We want to track the name and version of the python packages we are using so we can recreate our environment easily in the production area. There's a pip command that does this:

	$ pip freeze > requirements.txt

I pushed the output of pip freeze to a file called requirements.txt which we will add to source control so we always have an updated list of packages being used.
	
As before, we commit the changes to git and push to remote repository.

### Install Fabric
To easily deploy a Django application, we use [Fabric](http://fabric.readthedocs.org/en/1.3.3/api/contrib/django.html). Fabric has a number of purposes but it really helps in quick deployments. Install Fabric with the command:

	$ pip install fabric
	
Fabric expects a fabfile.py which defines all of the actions we can take. Let's create that file and put the following in fabfile.py in your project's root directory.

	from fabric.api import local

	def prepare_deployment(branch_name):
    local('python manage.py test django_project')
    local('git add -p && git commit') # or local('hg add && hg commit')

This will run the test and commit your changes only if your tests pass. A simple "pull" in your production area becomes your deployment. Let's add the following code to fabfile.py to actually deploy:

	from fabric.api import lcd, local

	def deploy():
    with lcd('/path/to/my/prod/area/'):

        # With git...
        local('git pull /my/path/to/dev/area/')

        # With Mercurial...
        local('hg pull /my/path/to/dev/area/')
        local('hg update')

        # With both
        local('python manage.py migrate myapp')
        local('python manage.py test myapp')
        local('/my/command/to/restart/webserver')
        
This will pull your changes from the development master branch, run any migrations you've made, run tests, and restart your web server.

To use this file, we simply type the following in production environment:

	$ fab prepare_deployment
	$ fab deploy

So that's it! The skeleton for an optimal django environment is setup and ready to be developed! Remember, commit often, test everything. Let me know if i've missed out something! Rock on folks!

P/S: For your convenience, i have setup a [django_template](https://github.com/hguochen/django_template) on github based on the above instructions. You may wish to fork it for your own use!

EDIT: Thanks to Chng Nai Yun, i've corrected a silly error which incorrectly states that git is installed when you install virtualenvwrapper. In reality, git is only installed when you setup the virtualenv environment.