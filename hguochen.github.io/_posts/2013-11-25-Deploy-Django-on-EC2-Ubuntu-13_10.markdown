---
title: "Deploy Django on EC2 Ubuntu 13_10"
layout: post
date: 2013-11-25 23:08:40
comments: true
categories: []
---
I've had a hard time wrestling with Django on an EC2 instance and after finally figuring it out, i'm documenting this down so hopefully it makes life easier for you.

1. if you haven't already, you have to sign up for [Amazon Web Services](). Then sign in, open the AWS Management Console, select EC2 instances and Launch instance of Ubuntu Server 13.10 64-bit.
2. Create a new Key Pair and download it. You will need it later. Click next and Launch the new instance.
3. Check the security group in the description of your EC2 instance. Go to security groups and edit the matching Security group of your instance. Select inbound and create a new rule for HTTP( port80 ) click Add, then Apply Rule Changes.
4. Connect your EC2 instance with ssh:

		$ ssh -i xxx.pem ubuntu@ec2-xx.xxx.xxx.xx-comput1-amazonaws.com
	
5. Once you are connected, run the following commands:

		$ sudo apt-get update
		$ sudo apt-get upgrade
		$ sudo apt-get apache2 libapache2-mod-wsgi
		$ sudo apt-get install python-pip
		$ sudo pip install django
		$ sudo apt-get install build-essential
		$ sudo apt-get install python2.7-dev
		$ sudo apt-get install git
		$ sudo apt-get install mysql-server
		$ sudo apt-get install python-dev libmysqlclient-dev
		$ sudo pip install MySQL-python
		
6. Now it's time to configure the apache configure file

		$ sudo vim /etc/apache2/apache2.conf
		
7. Put the following code at the end of the file:

		WSGIPythonPath /path/to/python/file
		ServerName website.com
		DocumentRoot /path/to/file
		Alias /static /path/to/file/static/
		Order deny, allow
		Allow from all
		WSGIScriptAlias / /path/to/file/wsgi.py
		Order allow, deny
		Allow from all
		
8. Enable apache virtual host

		sudo a2ensite yourhost
		
And your site is up! Quickly go check it out at http://yourdomain.com
	

