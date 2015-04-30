---
layout: post
title: "A ProXPN Client for Linux"
description: "Introduction to the command line client I wrote to simplify the use of ProXPN's OpenVPN service on Linux."
category: tools
tags: [VPN, Github, crypto, Linux, Arch, OS X, OpenVPN]
---
{% include JB/setup %}

At the end of 2014 [I switched to Arch Linux from OS X](https://mattsurabian.github.io/arch-linux-on-a-macbook-81/). I'd though about making the switch before, and for the first time everything lined up to make it so. All of the programs I used on a daily basis would run fine or had solid alternatives available. I had to give up a few programs I only used occasionally, and for the most part they haven't been missed. [ProXPN](https://proxpn.com/) was the exception. 

### Why Bother with a VPN Service?
Because I'm a consultant, I often need to join networks that are untrusted or monitored while traveling for work. I really missed being able to use my subscription because a Linux client wasn't available. Further, the cost of a subscription (especially with my [Security Now](http://twit.tv/show/security-now) discount) was less than the cost of running my own OpenVPN server, to say nothing of providing multiple exit nodes. While there are lots of OpenVPN service providers out there, [ProXPN](https://proxpn.com) supports the [TWiT Network](http://twit.tv) which I'm a fan of, so I was inclined to avoid switching providers if I could avoid it.

### Something Is Better Than Nothing
I knew that under the hood ProXPN's client software was using OpenVPN, so I suspected it would be possible to get the `.ovpn` configuration file and use OpenVPN to connect to the exit node servers directly. This suspicion turned out to be correct and I was able to extract the configuration file from the OS X package. By studying the OpenVPN documentation I was able to quickly get up and running once I received a list of exit nodes from ProXPN customer support. 

Ultimately though, my solution was very much a manual process. I had all the remote exit nodes on commented out lines in a modified configuration file with only the remote node I wanted to use un-commented.  Every time I wanted to connect over VPN I needed to make sure the exit node I wanted to use was un-commented, ensure I didn't accidentally break the formatting of the configuration file, and make the call to OpenVPN. Not difficult, but cumbersome for sure.

### Lowering the Barrier for Usage
That's why I decided to write a command line client to make structuring the OpenVPN command simple and less prone to typos and human error. I used bash because it seemed the most compatible option across all distributions without introducing any additional language dependencies. The readme contains [a plain language summary of exactly what the script is doing](https://github.com/MattSurabian/proxpn-bash-client/blob/master/README.md#how-does-this-script-work) in case your bash is rusty, but at a high level: the shell script ensures OpenVPN is installed, interactively prompts the user to select which exit node should be used, and assembles the OpenVPN command. Before trying to connect, it shows the command to the user and waits for confirmation unless the `-y` flag is passed.

<script src="https://gist.github.com/MattSurabian/8df6949b7a0bb8d3677b.js"></script>

You can [read about how to install and use the ProXPN Bash Client in the readme](https://github.com/MattSurabian/proxpn-bash-client/blob/master/README.md), [download a zip file of the latest release](https://github.com/MattSurabian/proxpn-bash-client/releases), or view the [liberally commented source code for the ProXPN Bash Client](https://github.com/MattSurabian/proxpn-bash-client/blob/master/proxpn) on [Github](https://github.com/MattSurabian/proxpn-bash-client).

If you find this helpful or have suggestions for improvements please let me know either [via Github](https://github.com/MattSurabian/proxpn-bash-client/issues), [email](mailto:matt@mattsurabian.com), or in the comments below.