---
title: "Update python2.6 to python 2.7 in ec2 linux instance"
layout: post
date: 2013-09-12 11:10:54
comments: true
categories: [Development]
---
This post by [Kurttheviking](http://tumblr.kurttheviking.com/post/30920138314/painless-python-2-7-on-aws-ec2) gives the steps in updating python 2.6 to python2.7 in AWS EC2 instances but i find the writeup abit messy and missing some considerations. So i'm reorganizing it in number sequence.

1. Check current python version

		$ python -V
		Python 2.6.8
		
2. AWS now provides python2.7 via the package manager, so we can install 2.7 easily.

		$ sudo yum install python27
		
3. Now there are 2 versions of python in your instance

		$ python -V
		Python 2.6.8
		$ python27 -V
		Python 2.7.5
		
4. Check where Python2.7 is expecting packages to go

		$ python27 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"
		/usr/lib/python2.7/site-packages
		
5. To enable easy_isntall with our fresh Python2.7 interpreter we need to add setuptools. Head on over to [Setuptools](http://pypi.python.org/pypi/setuptools) and confirm the URL for the egg installer. Then, change back to your cozy ec2-user home directory and snag a copy of that egg file.

		$ cd ~
		$ wget http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11-py2.7.egg#md5=fe1f997bc722265116870bc7919059ea
		
6. Run the egg as you would any shell script

		$ sudo sh setuptools-0.6c11-py2.7.egg
		Processing setuptools-0.6c11-py2.7.egg
		Copying setuptools-0.6c11-py2.7.egg to /usr/lib/python2.7/site-packages
		
7. Add git package

		$ sudo yum install git
		
8. Set alias in .bash_profile for the new python2.7

		$ sudo vim .bash_profile
	Add the following line to .bash_profile

		alias python='python27'
		
	Exit vim and source the file

		source .bash_profile
		
**Note**: If you set python2.6 as the default python interpreter, yum might break as a result. You can follow [this guide](http://freethreads.wordpress.com/2013/06/19/fixing-broken-yum-on-cent-os-5-9/) to fix 'No module named yum' error