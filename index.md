---
layout: page
title: Showing machines who's boss since 1984.
tagline:
---
{% include JB/setup %}

## About
I'm a web developer living in Massachusetts and working remotely on a global development team for [Ceros](http://www.ceros.com), a powerful, collaborative, real time, cloud based publish everywhere solution for interactive content experiences.

I'm interested in clean code, security, and collaborative realtime technologies. I believe truly great ideas require an amazing team and a strong DevOps foundation.  I usually write about web technologies, and am passionate about my craft.


## Latest Post
{% assign first_post = site.posts.first %}
### <a href="{{ BASE_PATH }}{{ first_post.url }}">{{ first_post.title }}</a>
{{ first_post.description }}

## Favorite Posts

### <a href="http://mattsurabian.github.io/how-i-test-locally-hosted-sites-with-physical-devices/">How I Test Locally Hosted Sites With Physical Devices</a>
A walk through of how I use Charles proxy to test locally hosted sites with physical devices

### <a href="http://mattsurabian.github.io/dot-vaults-encrypted-hidden-file-backup/">DotVaults: Encrypted hidden file backup</a>
An outline of my dot-files project and how it can help to backup hidden configuration files

### <a href="http://mattsurabian.github.io/how-and-why-im-using-rackspace-and-opscode-hosted-chef/">How and Why I'm Using Rackspace and OpsCode Hosted Chef</a>
I recently went through the exercise of migrating my old Debian servers on Rackspace configured using a shell script to the new Rackspace OpenCloud with OpsCode hosted Chef. This post outlines what Chef is, why I’ve chosen to utilize it, and provides a link and introduction to my github hosted rackspace-kitchen.chef repo

### <a href="http://mattsurabian.github.io/eliminating-confirmation-bias-putting-the-science-back-into-computer-science/">Eliminating Confirmation Bias: Putting the Science Back Into Computer Science</a>
Confirmation bias is the tendency to favor information that supports one’s views or hypothesis. Every time you’re troubleshooting a problem and think, “I know it can’t be ________” you might be guilty of it. As a web developer I know that feeling all too well. Think about your last debugging experience...

### <a href="http://mattsurabian.github.io/my-off-grid-power-setup/">My Off Grid Power Setup</a>
A writeup on the off grid power setup I have, how it works, and a general overview on what you need to know to build one yourself.


## Older Posts

<ul class="posts">
  {% for post in site.posts limit:15 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
