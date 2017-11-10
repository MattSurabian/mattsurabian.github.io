+++
date = "2017-10-30T17:00:00+09:00"
title = "Use Docker: Isolated Ad Hoc Dependencies"
description = "This post is about how I use Docker to handle project dependencies to increase my efficiency and give me more room to experiment."
thumbnail = "images/container-crane.jpg"
+++

This post is [part of a series about using Docker in your development workflow]({{< ref "posts/2017-10-25-how-docker-improves-my-workflow.md" >}}) even if you’re not ready to start shipping containers into production. Whether you’re just getting started with VMs and Vagrant, have avoided Docker because it seems too complicated, or are just looking for ideas on how to use Docker more, this series covers some of the things that helped me start loving it.

## My First Aha Moment
About two years ago I was working as a consultant with an application I had never run locally before and it needed a 
Redis server. In the production environment, this server was completely controlled by another team and all I knew was 
that the application needed a valid Redis connection string to do its work. Rather than install redis locally or set-up 
an entire Vagrant box just for one random dependency I decided to run:

```
docker run -d -p 6379:6379 --name my-app-redis redis
```
In *less less than a minute* I had a local Redis server available at `localhost:6379` ready to be connected to that was 
totally isolated from my machine and was able to be brought up and down essentially instantly. I also realized I was able 
to easily switch between different versions of Redis and the underlying Linux distribution by slightly altering my Docker 
command; no more having to Google the right shell incantations necessary to get things up and running in my own Vagrant 
box. 

Best of all, [the Redis maintainers took care of putting the container together for me](https://hub.docker.com/_/redis/) so I knew it was configured 
properly. I still love Vagrant, but the trusted ecosystem for base boxes is nothing compared to Docker; add to that the 
instant availability of a container and the whole thing is just too good to pass up.

## Docker Hadn’t Really Changed I Had
Docker hadn’t really changed much since the last time I had played around with it; I was just in a better place to give 
things a shot. I ran into a real world problem that was blocking me from getting things done and I was able to use Docker 
to solve it quickly without a lot of ceremony. Suddenly I understood what everyone was so excited about. 

In future posts I’ll go into more detail about setting up specific configurations for dependencies like ElasticSearch and 
Redis which I’ve used many times; but the official documentation for many containers usually covers how to tune the 
settings or start with seed data.

## Task Runners, Make, Something!
One thing that can help a lot is wrapping some of this stuff with a task runner of some kind. That can be as simple as 
`npm` scripts in JS projects, `Make`, or even a `bin` [folder with scripts that handle the lifecycle of dependent containers](https://github.com/MattSurabian/mass-keno-tracker-api/tree/master/tasks). 
This will help others on your team start working with Docker without feeling like they need to learn how every flag works.

## When To Think About Trying Containers Instead of a Traditional VM

**Front-end applications that need persistent data stores**
 
This is a great place to start with containers. I know plenty of folks running Vagrant machines just to provide postgres to an application and it makes no sense. If I had a dollar for every time I heard, “it builds on my machine but not in the VM” or my personal favorite, “something is going on with canonical right now and my Vagrant up is hung”... woof.

**Any application where a dependency is a black box**
 
Usually this occurs when something is totally controlled by another team or outside vendor.

**To simulate or troubleshoot single points of failure**

I’ll go into this more in a future post, but if your application implements any kind of fallback state or graceful error handling being able to test and demo that with containers is so much faster than with traditional VMs.

**Any time a dependency is configured but not installed by you/your team**

I’ve worked in plenty of restricted network environments where an application team ends up only being able to ship configuration instructions to another team that manages the servers. Practicing in a container is a great way to ensure your team’s understanding of the current configuration stays fresh instead of letting a pet VM or shared dev environment sit around and get stale.

Hopefully this has convinced you to give Docker a try, if you're interested in learning more the next post in this series is 
about [ensuring a totally consistent and clean environment for building, compiling, and testing software across a diverse team.]({{< ref "posts/2017-11-9-use-docker-clean-room-style-builds-and-tests.md">}})
