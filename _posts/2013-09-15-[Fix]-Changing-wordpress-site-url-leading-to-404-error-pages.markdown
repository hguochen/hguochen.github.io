---
title: "[Fix] Changing wordpress site url leading to 404 error pages"
layout: post
date: 2013-09-15 11:38:14
comments: true
categories: [Development]
---

This is actually an easier problem than [Wordpress Permalink not working](http://blog.hguochen.com/blog/2013/09/fix-wordpress-permalink-not-working/)

1. In terminal, go to your wordpress folder directory

		$ cd /path/to/wordpress_directory/

2. Open wp-config.php file

		$sudo vim wp-config.php

3. There will be a long list of defines already written in the file, like the following:

		define('AUTH_KEY', 'put your unique phrase here');
		define('SECURE_AUTH_KEY', 'put your unique phrase here');
		define('LOGGED_IN_KEY', 'put your unique phrase here');
		define('NONCE_KEY', 'put your unique phrase here');
		define('AUTH_SALT', 'put your unique phrase here');
		define('SECURE_AUTH_SALT', 'put your unique phrase here');
		define('LOGGED_IN_SALT', 'put your unique phrase here');
		define('NONCE_SALT', 'put your unique phrase here');

	Add the following defines just below the last define:

		define('WP_HOME','YOUR_HOME_URL');
		define('WP_SITEURL', 'YOUR_HOME_URL');
		define('RELOCATE',true);

This is the simplest method to fixing this problem, but the drawback is that your website url is now hardcoded. You will not be able to dynamically change the url in wordpress dashboard **Settings** tab anymore.

If you are looking for a more robust solution, [this](http://codex.wordpress.org/changing_the_site_url) will help you out.