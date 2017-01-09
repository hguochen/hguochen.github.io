---
title: "Beebeta Engineering"
layout: post
date: 2014-03-31 16:19:30
comments: true
categories: [Web development]
---

[Agus](https://www.facebook.com/leeagustiadi) and [Joel](https://www.facebook.com/jaielhaevin) conceptualized the Beebeta project sometime ago and invited me to handle the technical aspect of it. I didn't know what Beebeta was then but the moment i heard of the idea, i loved it! We spent our valuable weekends hacking out Beebeta and we are going to release it very soon! [Beebeta v1.0](http://www.beebeta.com) is finally out for public thrashing and hating! Ok but please be nice :(

Having architected the Beebeta backend, i thought it would be fun to give an idea of Beebeta from an engineering perspective. This is how we think an online polling system should be built. Feel free to disagree, we would love to hear your opinions :)

In short, Beebeta is a web platform to gather crowdsourced knowledge. It is an online timeless repository of public opinions which is easily accessible through a poll-like web interface. 

### Why Beebeta?
Search engines are vast, and non-targeted. While you will definitely be able to find valuable facts and informations on topics, it is not built to reflect the public's opinions on questions at hand. That's what Beebeta is for. It gives you insights to what the crowd thinks. Which could be questions as trivial as 'Which restaurant serves the best Ramen?' or global issues such as 'Should gay marriage be allowed?'.

Ok so much for pitching, that's not the purpose of this post. We are here for the nerd stuff arn't we?

### Engineering Goal
Throughout the implementation of Beebeta, we had a few goals in mind:

1.	Don't do unnecessary stuff
2.	Don't reinvent the wheel
3.	Be versatile in technology options

### OS/Hosting
Beebeta runs Ubuntu 13.10(Saucy Salamander) on Amazon EC2. We tried Linux AMI(CentOS) on EC2 but find the environment not as usable as Ubuntu, plus the software updates were not consistently updated. Also, stress tests on the CentOS server resulted in unpredictable crashes. Since Beebeta is a small project, we ruled out self-hosting as an option. We definitely don't want to waste time setting up servers ourselves.

### Load Balancing
All server requests goes through our load balancers. We use Amazon's Elastic Load Balancer(ELB) with 2 Nginx instances behind it. I admit now may be abit too early to devote load balancing resources but in case of unexpected traffic spikes, it will be good to pick up from here and scale instead of getting caught off-guard. I skipped using anything to handle the DNS though. But i may revisit the issue with Amazon's Route53 in future.

### Application Server
[Python 2.7.5](https://www.python.org/download/releases/2.7.5/) and [Django 1.6.2](https://www.djangoproject.com/) was used on Amazon micro-instance machines. To encourage decoupling with the machine itself, we used [virtualenv](http://www.virtualenv.org) to sandbox the development environment. As such, the machines become stateless and we can easily spring up more instances and deploy direct when it comes to scaling up. Dedicated IP is also allocated to the instance. I'm guessing that most of the requests would be CPU-intensive than memory-intensive, so at the moment, a micro-instance would suffice.

Mod_wsgi is used as our WSGI server, together with Apache and Mysql. A typical LAMP stack. I'm exploring the possibility of shifting to [Gunicorn](http://gunicorn.org) in future. I read some tutorials on gunicorn and found it easier to configure. But the standard mod_wsgi is good for now.

### Data storage
Meta-data's such as tags, url strings etc lives in MySql table rows.

Our BLOB data lives in Amazon's S3 standard storage. I thought of using glacier storage for original files and standard storage for rendered files but unfortunately S3 does not provide glacier storage in Singapore region yet. I forsee using glacier storage would cut costs by about 30% for every 10GB of data. Will keep in view for future changes, once glacier storage is opened up in Singapore. Content Distribution wise, we use [Amazon CloudFront](http://aws.amazon.com/cloudfront), which helps with page load times from users at different parts of the world.

Caching wise, we use memcached and currently have 1 memcached instance, which is connected to using pylibmc & libmemcached. I'm aware there's [Amazon's Elastic Cache](http://aws.amazon.com/elasticache/) service for this, but i havent got around to reading up on that yet.

### Django apps
As part of our goal not to reinvent the wheel. we used quite a few out-of-the-box django apps to facilitate building of Beebeta platform.

[S3Boto](http://boto.readthedocs.org/en/latest/ref/s3.html) is used to connect the server scripts with different Amazon web services seamlessly.

[Django-analytical](http://pythonhosted.org/django-analytical/services/mixpanel.html) is used to track various site metrics on the application itself.

[Django-debug-toolbar](https://pypi.python.org/pypi/django-debug-toolbar)(my favourite debugging tool!) is used on development environments for almost all debugging tasks.

[Django-pagination](https://pypi.python.org/pypi/django-pagination) is used to paginate database querysets into proper pages.

[Django-south](http://south.readthedocs.org/en/latest/) is used to easily perform database migration tasks between ORM and the database itself.

[Django-registration](https://bitbucket.org/ubernostrum/django-registration/) is used to handle user registration/login/logout issues.

### Monitoring
We are using [Mixpanel](https://mixpanel.com) to track user activites on the site. These tracking statistics are filtered up to 2 timeless campaigns. First campaign to track site traffic activites such as page traffic, user sign ups etc. Second campaign to track question and choices interactions of all questions on the site. At any one time we will know which questions are most popular or which group of users are most active at different times of the day.

For backend monitoring, we are using [New Relic](http://newrelic.com) to track application performance and server performance. We establish a certain threshold for performance issues on both of these assets and issue warning emails to administrators when the threshold is almost reaching.

For Python error reporting, we use [Sentry](http://sentry.readthedocs.org/en/latest/), an open-source Django app written by the [Disqus](http://disqus.com) folks. We can closely monitor error messages at real-time relatively easy with this handy tool.

### What's your stack?
That's so much for Beebeta stack. Feel free to leave your comments below if you think we've made a terrible mistake. Otherwise, I'd love to hear how you implement your stacks!

Lastly, i've been playing this song on repeats all day in my coding time. It just energizes me for the tasks at hand. Hope you like it too! :)

{% youtube c_H3MWVx6JU %}