---
layout: post
title: "Moving My Blog To Jekyll"
description: "Dear WordPress: It's not you, it's me.  Actually… scratch that, it's you."
category: Web Development
tags: [blogging, performance, web development, scale]
---
{% include JB/setup %}

Performance At Scale
----------------------
WordPress is an incredible blogging platform and the strategy for deploying WordPress at moderate scale is pretty straightforward: a horizontally scaling web array and a vertically scaling database.

The problem is that I (and I imagine the majority of people with industry blogs) don't need constant scale.  A "scaling event" might occur once every few months.  I refuse to sacrifice ability to scale for convenience or cost.  This weekend I realized I didn't have to choose.

What I Learned From [HowSecureIsYourTwitterPassword.com](http://www.ismytwitterpasswordsecure.com/)
-----------------------------------
[Alastair Coote](https://twitter.com/_alastair) Created a really awesome web page that cautioned users to be vigilant when websites prompt them for login credentials after the unfortunate [Associated Press twitter hacking incident](http://static.ddmcdn.com/gif/blogs/dnews-files-2013-04-fake-ap-tweets-jpg.jpg) that caused a [flash crash on the NYSE](http://money.cnn.com/2013/04/24/investing/twitter-flash-crash/index.html).  [Alastair was able to perform at scale on a shoestring budget](http://blogging.alastair.is/how-i-served-100k-users-without-crashing-and-only-spent-0-32/) thanks to the magic that is [Amazon S3](http://aws.amazon.com/s3/).  

Alastair's ability to meet demand on a budget really made me question my dependency on MySQL and WordPress.  While there are lots of people that require the flexibility and approachability of the WordPress platform I'm not one of them.  I wanted to be able to serve static files without needing to roll my own blogging platform.    

How I Came To Love [Jekyll](http://jekyllbootstrap.com/)
-------------------------
Jekyll provided me everything I wanted in blogging engine.  I can write posts in markdown using [Mou](http://mouapp.com/), experiment locally using the Jekyll gem, and publish using version control.  Because Jekyll generates static files scaling is possible for free via [GitHub Pages](http://pages.github.com/) or cheaply by publishing to [Amazon's S3](http://aws.amazon.com/s3/).

[Jekyll's quick-start guide](http://jekyllbootstrap.com/usage/jekyll-quick-start.html) makes it pretty easy to get up and running, posts and pages are created with rake tasks and publishing is as simple as pushing local changes to GitHub.

Goodbye, WordPress
-----------------
I won't miss the way you absolutely [required a caching layer for reasonable performance](http://codex.wordpress.org/High_Traffic_Tips_For_WordPress).  Or the way you constantly screwed up my code snippet formatting.  I'll laugh when I think back to the the time I migrated your data using SSH and a well constructed linux pipe command, and I'll cry when I think about [how vulnerable you were to any slag with an active internet connection](http://krebsonsecurity.com/2013/04/brute-force-attacks-build-wordpress-botnet/).  

It's been a good ride but we're through.  I'll continue to talk you up to our mutual friends, but my heart belongs to another.

OMG [Markdown](http://en.wikipedia.org/wiki/Markdown)
------------
It's just great.  Seriously.  Futuristically awesome.  Maybe I'm easily impressed but it seems to me the lightest weight most useful modern content creation syntax available.  If you write for the web and you haven't experienced it take the time to do so.

     
The Migration Process
---------------------
 - Prerequisites: [GitHub](https://github.com) account, [Disqus](http://disqus.com/) account if you require commenting
 - Follow the [Jekyll quick-start guide](http://jekyllbootstrap.com/usage/jekyll-quick-start.html).  It seriously is this simple.
 - [Pick a classy theme](http://themes.jekyllbootstrap.com/) and modify it, or make your own theme.
 - Migrate your pages to Markdown (maybe there's a shortcut here, I just did it by hand)
 - Setup [301 Redirects](http://support.google.com/webmasters/bin/answer.py?hl=en&answer=93633)
  

  



  