---
layout: post
title: "How and Why I'm Using Rackspace and OpsCode Hosted Chef"
description: ""
category: 
tags: []
---
{% include JB/setup %}

_I am not affiliated in any way with Rack Space or OpsCode, I’m just a user that loves the flexibility it offers me. <br/>I am a LAMP stack developer that uses Ubuntu._

What is Chef?
--------------
 - Chef is an open source configuration management tool written primarily in Ruby.
 - Chef is a tool used for DevOps that allows for the provisioning and creation of cloud services and servers using ruby and JSON.

Don’t Know Ruby?
--------------
Don’t even worry about that! There are so many resources available on github that leveraging chef doesn’t require fluency in Ruby. For developers familiar in a different language Ruby syntax is not so cryptic it can’t be learned relatively quickly; at least within the context of Chef cookbooks.

Why Chef?
--------------
 - Chef because infrastructure is just as important to success as content
 - Chef because boot time from bare-metal, consistency, and stability all matter.
 - Chef because version control is awesome and makes our lives easier.
 - Chef because managing infrastructure with the same best practices one manages a codebase with creates a fundamentally more stable and transparent architecture that can be understood by perusing a git repo and chomping down on some readme files instead of ssh’ing into a box and poking around for hours.
 - Chef because… Seriously, has it not yet occurred to you how awesome automated versionable server configuration is?!?!

Scale is for Enterprises, why should I bother
--------------
Personally, I’m passionate about web development and everything that we as a community can accomplish in the cloud.  To echo the main point of my previous post: Why Don’t You Have a Rackspace Account, I can’t help but want more hands on experience with the technologies I depend on to make my living.

I love tinkering with various personal projects and having the ability to define and bring up custom configured servers in minutes using Chef speeds up my development process and makes me feel more professional.  Add to that the option to distribute my environment along with the source: sign me up.

In the event a project catches fire, if only briefly, I’m prepared to scale horizontally instead of crashing like a chump.  In my twisted world view this effort makes a difference to your audience.

A side note to all you wanna-be-tech-entrepreneurs, if you don’t know how this scaling thing works, you’re not prepared to succeed in the tech industry.  Successful ideas require performance at scale.  Ignoring infrastructure completely or putting it off until later is a recipe (get it!? Chef!? recipe!?) for disaster.  Embracing a method of consistent automated deployment is an absolute necessity.  Chef is an amazing tool to accomplish just that.

A Tangent on Server Images: Debian vs Ubuntu LTS
--------------------------------------
I’m a long time Debian user, but I’ve recently made the switch to Ubuntu 12.04 LTS from Debian 6.0.  The reason for this is simple and boils down, not to some terrible experience I had with Debian; but to three letters: L-T-S.  Long.  Term.  Support.  Let me break it down for all the haters out there: LTS means 5 years guaranteed support.  Support as in security patches.  Support as in I know for sure the servers I launch today will be just as secure, relevant, and stable in 2018.  Debian, while incredibly stable, is not on a fixed release schedule.  Old versions are supported for one year after a new version is released.  With a new version rumored to be released soon, it’s unclear when 6.0 would become deprecated; making Ubuntu 12.04 LTS my image of choice.

Come on in my kitchen
----------------------
I’ve posted my rackspace-kitchen.chef repo on github to serve as a resource for others just getting into this crazy Chef thing.  It’s a very simple Chef repo and the README files explain usage.  If this will be your first time working with Chef you’ll need to prepare your workstation.  The following resources will get you ready to clone and work with any Chef repo including my rackspace-kitchen.chef repo which will have you spinning up a standardized LAMP server, dedicated apache/node.js box, or a dedicated mysql/redis box.

ReadMe Documents
-----------------
[Main Rackspace Kitchen Readme](https://github.com/MattSurabian/rackspace-kitchen.chef/blob/master/README.md)

[Rackspace Kitchen Roles Readme](https://github.com/MattSurabian/rackspace-kitchen.chef/tree/master/roles)

[Rackspace Kitchen Custom Config Cookbook Readme](https://github.com/MattSurabian/rackspace-kitchen.chef/tree/master/cookbooks/custom-config)


Resources to Start Using OpsCode Hosted Chef with Rackspace
----------------------------------------
[Setting Up Your Workstation via OpsCode](http://docs.opscode.com/install_workstation.html)

[Cooking With Chef Part 1 via Rackspace](http://devops.rackspace.com/cooking-with-chef.html#.UTQPdxk85uo)

[Cooking With Chef Part 2 via Rackspace](http://devops.rackspace.com/cooking-with-chef2.html#.UTQPhRk85uo)