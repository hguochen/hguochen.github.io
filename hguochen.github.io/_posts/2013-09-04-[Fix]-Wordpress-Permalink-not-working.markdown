---
title: "[Fix] Wordpress Permalink not working"
layout: post
date: 2013-09-04 11:46:21
comments: true
categories: [Development]
---

There has been many solutions on the web with regards to this issue. I'm drawing exerpts from some of these links which worked for me.

Problem: When you try to use fancy permalinks in the **Settings>Permalinks** under Wordpress Dashboard, you may encounter broken links to each individual posts.

1. Does .htaccess file exist?

Whenever you change the Permalinks, Wordpress should update the .htaccess file for you. Sometimes if the file is not created, Wordpress won't create it automatically. This is simply because it may not have the permission to do that. So if you can't find the file in the root of the Wordpress installation (After showing the hidden files), create it yourself.

2. Is .htaccess file editable?

If the file is there, check the modification time of the file. Then change the Permalink settings from WordPress hit save and go back to check the time again. If it’s not updated then WordPress doesn’t have the permission to update it. To update it you can do **one** of the following:

- Change the permission of your htaccess to 777(or whatever works in your server), and set the Permalink again. Once the wordpress added the required code in .htaccess, change back the permission of .htaccess to 644.
- Add the following code to your .htaccess file manually:

		<IfModule mod_rewrite.c>
            RewriteEngine On
            RewriteBase /
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteCond %{REQUEST_FILENAME} !-d
            RewriteRule . /index.php [L]
            </IfModule>
            # END WordPress

3. Is rewrite Module enabled in your server?

If you are in a shared server, simply ask the technical supports whether this module is enabled. If you are on your server try running this command in your terminal:

		$ sudo a2enmod rewrite

If you get "Module rewrite already enabled" then move to the next step or else restart your apache server using:

		$ sudo service apache2 restart

4. Does apache know about .htaccess file?

Open apache2.conf file (normally you will find it in /etc/apache2) and make sure that this line is there:

		AccessFileName .htaccess

5. Search for AllowOverride option

Try searching for "AllowOverride" in these files:

- /etc/apache2/apache2.conf
- /etc/apache2/httpd.conf
- /etc/apache2/sites-enabled/000-default
- /etc/apache2/sites-available/yourdomain.com

If you found it in this form:

	AllowOverride None

just comment it out by adding # in front of it. Then restart Apache and check if the Permalinks are working.

6. Make sure that the Permalinks custom setting is correct

If you are using Permalinks custom settings make sure that /index.php/ is **not** written in front of your Permalink.

7. Does the server allow the symlinks?

Try adding the following code to the top of your .htaccess file:
	
	Options+FollowSymlinks
	RewriteEngine on

These are the checklists i used when trying to fix the issue. Additionally, setting the 'AllowOverride' option in apache2.conf did not work for me but the following worked.

	$ sudo vi /etc/apache2/sites-available/default

change:

	<Directory /var/www/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride None
                Order allow,deny
                allow from all
    </Directory>

to:

	<Directory /var/www/>
                Options Indexes FollowSymLinks MultiViews
                AllowOverride ALL
                Order allow,deny
                allow from all
        </Directory>

Restart apache:

	$ sudo /etc/init.d/apache2 restart

Also check your .htaccess file, for some reason my generated .htaccess had the wrong RewriteBase

My folder is called wordpress under /var/www, so i changed the RewriteBase in .htaccess to

	RewriteBase /wordpress/

Lastly, in ubuntu servers, there's an easier way to enable mod_rewrites

	$ sudo a2enmod rewrite
	$ sudo service apache2 rewrite

Extracted from:
[http://www.felfelworld.com/2013/02/12/page-not-found-permalink-wordpress/](http://www.felfelworld.com/2013/02/12/page-not-found-permalink-wordpress/)
[http://wordpress.org/support/topic/new-installation-of-wp-331-permalinks-dont-work](http://wordpress.org/support/topic/new-installation-of-wp-331-permalinks-dont-work)