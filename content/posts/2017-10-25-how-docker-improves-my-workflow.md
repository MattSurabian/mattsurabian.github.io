+++
date = "2017-10-25T17:00:00+09:00"
title = "3 Ways Docker Has Improved My Development Workflow"
description = "Some things I started using Docker for that helped me go from not really understanding or liking it to loving it."
thumbnail = "images/docker-party.png"
+++

When Docker came out four years ago it seemed to me like complicated virtual machines with an obtuse workflow, and like 
a fool I decided it didn’t seem worth the trouble. I should have taken it as a sign that people way smarter than me like 
[Jessie Frazelle](https://blog.jessfraz.com/) were all over containers and doing really cool things right with them right out of the gate. Instead, I 
buried my head in the sand, thinking that containers were “too much for my needs”. This was a mistake on my part, but 
thankfully one I’ve since recovered from. This series will share a few ways I started using containers that helped me 
see how great they are.

## The Before Time: Virtual Machines And Doing It Live 
The first time I ever used a virtual machine for work was probably in the mid 2000’s. At the time I was a die hard Mac 
user that occasionally found myself needing to test things in Windows or run some piece of random software; which meant 
firing up VirtualPC or Parallels so I could run Windows XP on my honkin 17” Macbook Pro laptop or old iMac. The utility 
of these tools was immediately clear to me and despite being a little rough around the edges they were easy enough to use.

A few years later, tools like VMWare Fusion and VirtualBox were now a regular part of my work day. Testing locally on a 
virtual machine was such a better experience than using MAMP and much more responsible than my old favorite workstyle: 
_editing files over FTP directly in production_. Yikes- but also sorry, not sorry.

So after a few more years, when Vagrant came out and allowed developers to manage and configure headless virtual machines 
in Virtualbox and VMWare Fusion from the command line, I was all over it. I already loved virtual machines and here was a 
tool that made them so much easier to use. I told everyone who would listen about my love for application isolation and 
the fancy new tool that was making my life easier.

## Then Containers Happened, And I Was Confused.
With virtual machines, the benefit was always immediately clear and easy enough to take advantage of. It seemed like suddenly 
I was reading and hearing about how containers were the future and how, “If you love virtual machines wait till 
you try Docker”. This was all really strange to me because I thought things had just gotten good with virtual machines; 
to say nothing of the fact that as a consultant I was still talking to lots of people who had yet to even try something 
like Vagrant.

## I Didn't Like Docker At First
I'll admit, I tried using Docker when it first came out and I got kind of overwhelmed. Even though by that time I was an experienced Linux
user and had even manually setup a chroot jail or two. Still, it was hard for me to see what the real benefit was compared to VMs, 
especially since it seemed more complicated to use and everything in our cloud environment was already a VM. Over the past year I've had
a few opportunities to experiment with Docker more and I'm now a full on kool-aid glugging convert. I owe this conversion 
to people like [Jessie Frazelle], [Kelsey Hightower], and [Bryan Cantrill]; they're out there talking about using this stuff, getting
people excited and making it seem approachable.

Over the next few weeks, this series will focus on the 3 things that I started doing with Docker that changed my mind about
using containers during development:

  - [Spinning up ad hoc dependent services (like Redis or Postgres) instead of going through all the work of configuring a VM.]({{< ref "posts/2017-10-30-use-docker-isolated-ad-hoc-dependencies.md">}}) Especially on projects where those dependencies were managed by a different team.
  - Ensuring a totally consistent and clean environment for building,compiling, and testing software across a diverse team.
  - Quickly simulating different data center environments to test networking and fail-over patterns in total isolation and at zero cost.

While I'll definitely be finding time to blog about scheduling, bin-packing, and security models in the future, I wanted to
start here in the hopes that I can convince folks who have come from a similar background and have been avoiding Docker to change their
minds and join the party.

<br/>

[Jessie Frazelle]: https://blog.jessfraz.com/
[Kelsey Hightower]: https://www.youtube.com/watch?v=NVl9cIiPF80
[Bryan Cantrill]: https://www.youtube.com/watch?v=xXWaECk9XqM
