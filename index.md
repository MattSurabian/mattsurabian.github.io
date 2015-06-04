---
layout: page
title: Showing machines who's boss since 1984.
tagline:
---
{% include JB/setup %}

## About
I'm an open web engineer living in Massachusetts and working for [Bocoup](http://bocoup.com), a technology company that helps move the web forward by contributing to open source technologies, training the community, and consulting.

I'm interested in clean code, security, and a successful, healthy deployment process. I believe truly great ideas require an amazing team and a strong DevOps foundation. I usually write about web technologies, and am passionate about my craft.


## Latest Post
{% assign first_post = site.posts.first %}
### <a href="{{ BASE_PATH }}{{ first_post.url }}">{{ first_post.title }}</a>
{{ first_post.description }}

## Favorite Posts

### <a href="https://mattsurabian.github.io/proxpn-bash-client/">A ProXPN Client for Linux</a>
Introduction to the command line client I wrote to simplify the use of ProXPN’s OpenVPN service on Linux.

### <a href="https://mattsurabian.github.io/arch-linux-on-a-macbook-81">Arch Linux on a Macbook 8.1</a>
My experience running Arch Linux on a Macbook Pro 8,1.

### <a href="https://mattsurabian.github.io/requirejs-projects-and-asynchronously-loading-the-google-maps-api/">RequireJS Projects and Asynchronously Loading the Google Maps API</a>
A pattern that is useful for handling dependencies which asynchronously load their own dependencies. Like google maps.

### <a href="https://mattsurabian.github.io/how-i-test-locally-hosted-sites-with-physical-devices/">How I Test Locally Hosted Sites With Physical Devices</a>
A walk through of how I use Charles proxy to test locally hosted sites with physical devices

### <a href="https://mattsurabian.github.io/dot-vaults-encrypted-hidden-file-backup/">DotVaults: Encrypted hidden file backup</a>
An outline of my dot-files project and how it can help to backup hidden configuration files

### <a href="https://mattsurabian.github.io/eliminating-confirmation-bias-putting-the-science-back-into-computer-science/">Eliminating Confirmation Bias: Putting the Science Back Into Computer Science</a>
Confirmation bias is the tendency to favor information that supports one’s views or hypothesis. Every time you’re troubleshooting a problem and think, “I know it can’t be ________” you might be guilty of it. As a web developer I know that feeling all too well. Think about your last debugging experience...

### <a href="https://mattsurabian.github.io/my-off-grid-power-setup/">My Off Grid Power Setup</a>
A writeup on the off grid power setup I have, how it works, and a general overview on what you need to know to build one yourself.


## Older Posts

<ul class="posts">
  {% for post in site.posts limit:15 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
