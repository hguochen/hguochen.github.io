---
title: "My jekyll workflow"
layout: post
date: 2014-01-08 16:48:29
comments: true
categories: [development]
---

I just automated my blogging with jekyll with the following routines. My goal is to cut down on inputting a string of commands into the terminal using [Rakefile](https://github.com/hguochen/jekyll-rakefile) and standard terminal commands.

>Jason Seifer has an awesome [Rake Tutorial](http://jasonseifer.com/2010/04/06/rake-tutorial) for developers new to Rake.

### My Github pages
I've set up [my github pages](https://github.com/hguochen/hguochen.github.io) to consist of two branches: source and master. The source branch consists of the entire project and as such tracks all changes. The master branch consists solely of the compiled site, which can be found in the _site subdirectory.

### My Local environment
blog/

|--blog.hguochen.com/

|--hguochen.github.io/

Main folder blog has 2 sub directories. blog.hguochen.com which holds jekyll generated site content. hguochen.github.io holds jekyll raw files such as _posts, _layouts, _includes etc.


### My workflow

Automate terminal commands by adding the following alias into .bashrc file:


	alias blogdir="cd /path/to/blog/hguochen.github.io"

	alias build_blog=""blogdir; jekyll build; sudo cp -r /path/to/blog/hguochen.github.io/_site/* /path/to/blog/blog.hguochen.com; cd /path/to/blog/blog.hguochen.com; git add --all; git commit -m "Blogpost on ["Date"]"; git push origin master;"

	alias bb="build_blog"

Move to jekyll raw files folder:

	$ blogdir
	
Create new post with Rake:

	$ rake post["Title"]
	
Previous command automatically fires up the default text editor for the file just created. In this case, i'm using [Mou](http://mouapp.com) for .markdown file extensions.

Once i'm done with my blog, i'm ready to build and publish my blog by pushing into github pages repository, with a simple alias we made earlier:

	$ bb
	
That's it! So this is my 3 step process to publishing my blog, powered by [Jekyll](http://jekyllrb.com).

Special thanks to [@leetucksing](https://twitter.com/leetucksing) for introducing me this awesome blogging tool! 

By the way, [@athanhcong](https://twitter.com/athanhcong) made an amazing app [BlogWith](http://blogwith.co/) which connects evernote to jekyll powered blogs and allows you to edit blogs directly on [Evernote](http://evernote.com). Gona try how it works with jekyll later :)
	
	

