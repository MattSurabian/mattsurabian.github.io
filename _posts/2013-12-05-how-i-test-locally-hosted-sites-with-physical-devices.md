---
layout: post
title: "How I Test Locally Hosted Sites With Physical Devices"
description: "A walk through of how I use Charles proxy to test locally hosted sites with physical devices"
category: Web Development 
tags: [web development, tips/tricks, devices, testing]
---
{% include JB/setup %}

Building web apps is totally awesome, especially when they work great in our multiscreen world. But behind the scenes to all kinds of amazing consistent functionality is a developer banging their head against the wall trying to figure out how to test things across all these different mobile devices locally, because the simulators aren't always good enough and pushing code for every little tweak and test is ridiculous.

That used to be me banging my head against the wall trying to solve this problem. Thanks to a lot of help from others over the years and some really cool tools my head is healed and I've arrived at my current solution. 

It's pretty simple, but before getting into the steps we need to talk a bit about networking.

## Prerequisites

#### Download <a href="http://www.charlesproxy.com/" target="_blank">Charles Proxy</a>

Charles is a super useful traffic monitor and proxy tool. [You can RTFM all about it](http://www.charlesproxy.com/documentation/), but you don't have to do that now. I'll cover the basics in this post.
 	
#### Determine your network environment

The network environment you're testing in will likely fall into one of these categories:

- A trusted home/office wifi and/or wired network where all connected computers are allowed to see/talk with each other: **Awesome, proceed to the first step:**

- A public/private wifi or wired network where all computers are not allowed to see/talk with each other: **You must create an ad-hoc network between your host machine and test devices before continuing.**

- A non-trusted wifi network where all computers can see/talk with each other. Like a public wifi hotspot with a jr. network admin: **Why are you connected to this kind of network at all!? You CAN proceed to first step and everything will work, but should probably checkout the ad-hoc network section if the entirety of what you're testing is available locally and you do not require an active internet connection.**

#### Setting up an ad-hoc testing network (optional headache) 

In order for you to test site with your mobile devices they either need to be on the same network, or the site needs to be on the public internet. Sometimes for security reasons networks won't allow traffic between machines on the network. This is a common setting for public wifi hotspots. So if you're in an environment like this and need to test a locally hosted site you'll need to setup an ad-hoc network. Otherwise you can proceed to step one.

In OS X this can easily be done from the "create network…" option in the wifi menu icon. Unfortunately, some Android devices cannot natively connect to ad hoc networks. Plus your host machine, unless it is hardwired via ethernet will lose its connection to wifi as the card is now broadcasting a signal instead of receiving it, meaning everything you need to test must reside locally.

If your host machine happens to be hard-wired you can leverage the "internet sharing" feature provided by many operating systems, which will allow you to share your wired internet connection by creating a wifi hotspot you could connect your mobile device to and be on your way to step one.

Some machines can be configured to share network connections with connected devices via USB as well, maybe worth looking into if you're unable to use the basic ad-hoc wireless network or internet sharing. 

Setting up ad-hoc networks can be a real pain, and rather than spend the rest of this post going on about it I'll leave it at this: *if your testing devices and your host machine aren't on the same network and can't communicate with each other none of the rest of this will work*.

## Step 1: Get your site's valid URL, or at minimum IP address

You can't test a web app without some kind of URL. I have three hosting scenarios for my local testing:

 - **Virtual Machine:** For work and larger projects I have virtual machines setup that get an IP address from my host machine (using NAT mode available in most virtualization products) allowing me to easily connect to it via a browser from my host machine only.
 - **MAMP, etc:** Great tool that I use for a lot of personal projects. MAMP is a self contained apache and mysql stack that runs locally on a configured port (the default is 8888). This setup produces URLs like `http://localhost:8888/` *Note that Charles wants to use port 8888 by default for its proxy. If you're using MAMP or port 8888 for some other reason, you'll need to either adjust the port you're depending on or change Charles' proxy settings to use a different port as discussed in step two.*
 - **Ad Hoc Web Servers:** Node, Python, and other languages are capable of hosting websites by creating ad-hoc servers listening on a configured port, these, like MAMP noted above give URLs of the form `http://localhost:PORT_NUMBER`
 
Most of the time I use /etc/hosts to assign valid domains to my IP address. Charles is capable of handling all the routing, meaning I wouldn't have to use /etc/hosts at all, but when I'm only testing locally vs using a device I don't want to always have Charles running. Charles also respects the entries within /etc/hosts so I don't have to deal with double config.

However, the big gotcha with /etc/hosts happens for users of MAMP and Ad Hoc Web Servers because /etc/hosts does not support port numbers! You could map your chosen test domain via /etc/hosts like this 

<pre>
127.0.0.1 awesometest.com
</pre>

 and connect to it at the end of this tutorial via `http://awesometest.com:PORT_NUMBER`; or you could just configure the port forwarding stuff in Charles, which we'll cover in the next section…

## Step 2: Setup Charles

Charles is designed to mostly set itself up during the install. If your machine is connected to the internet when you launch Charles you should be able to see traffic trickling through by clicking the "sequence" view. This is all your machine's http/https traffic, and Charles lets you do all kinds of cool stuff with it. But what we're really interested in here is Charles' ability to proxy and map.

### Proxy Port

In the menu bar checkout the "proxy" menu's "proxy settings…" option. **By default Charles' proxy is available at `http://localhost:8888` but if you need that port for your application or for MAMP you'll have to change this setting.** If you get a warning about the port already being in use when you start Charles, you may have to manually turn on the proxy once you've changed the port number. This is done from the "proxy" menu as well; when enabled in OS X a check is shown next to "Mac OS X Proxy".

For the rest of this tutorial I'll be referring to this configured port value as the **Charles proxy port**.

### Mapping

If you're using MAMP, an ad-hoc web server, or just don't want to deal with /etc/hosts as noted above you'll need to setup Charles to pass requests for your chosen testing domain name to your locally hosted site. This is accomplished by using Charles' menubar and selecting "tools" and "map remote". 

You can add new rules in here that tell Charles where to send traffic based on the hostname, protocol, and port. The basic settings for an http site hosted locally on port 8080 that you wanted to access using a browser via charlestest.com would be:

<pre>
Map From:
  Protocol: http
  Host: charlestest.com
  Port: 80
  Path: LEAVE_BLANK
  Query: LEAVE_BLANK
Map To:
  Protocol: http
  Host: localhost
  Port: 8080
  Path: LEAVE_BLANK
  Query: LEAVE_BLANK
</pre>

These rules can be turned on an off using the checkboxes next to them in the main "map remote" view. Wildcards can be used in the host name but remember a host value of `*charlestest.com` will match `www.charlestest.com` and `wowholycowcharlestest.com`. That may not be desired behavior depending on the brevity of your testing URL. I usually just have two entries: one for `www` and the other without.

## Step 3: Determine your host machine's local IP address

This is the IP address of your test computer as far as the local network is concerned. You can find this out from system settings or a command prompt. If your machine is connected to the internet *DO NOT* go to `whatismyip.com` because that's you're WAN IP and not what we need for this test.

From here on out I'll refer to this as your **LAN IP**

## Step 4: Configure your mobile device to use Charles' proxy

At this point you should be able to access your mapped domain via a browser on your local machine where Charles is running and everything should be working. Now the magic of getting your mobile device to use your machine as a proxy. All the networking headaches mentioned in earlier in the article have built up to this point. If your mobile device and your host computer running Charles are on the same network and can talk to one another, you can go into your devices network settings:

**Android:** The simplest way to set proxy settings is to go to the list of wifi networks, long press on the network you're connected to, select "modify network", and then use the "show advanced options" checkbox to reveal a "proxy settings" section, change the selection from "none" to "manual" and enter the following information:

<pre>
Proxy hostname: LAN IP (from step three)
Proxy port: Charles proxy port (from step two)
Bypass proxy for: LEAVE_BLANK
</pre>

**iOS:** Go to settings and get to your list of wireless networks, click the blue arrow next to the wireless network the device is connected to. Scroll down to the "HTTP Proxy" section and select the "Manual" tab. Enter the following information:

<pre>
Server: LAN IP (from step three)
Port: Charles proxy port (from step two)
Authentication: Off
</pre>

When you open a web browser on your phone after saving these settings Charles should pop up with a warning, saying a client is trying to connect to its proxy and requesting permission for this action. Allow it, it's your phone (hopefully)! Now all your phones traffic will be routed through your local machine, be displayed in Charles, and be subject to the mapping rules setup in step two and any /etc/hosts settings on the machine.

**Don't forget to set your proxy settings back to none when you're done testing. Otherwise when Charles is closed your device will no longer have an internet connection.**

## Step 5: Celebrate

That's it! Now read the [Charles manual](http://www.charlesproxy.com/documentation/) and find out about all the other stuff you can do with it! 

