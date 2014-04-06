---
layout: post
title: "Always Be Deploying™"
description: "There is a dirty little secret waiting to be discovered by anyone with a great idea: Deployment. Regardless of how amazing your functional prototype is it doesn’t amount to anything if it can’t be deployed and successfully scaled."
category: Perspective
tags: [web development, tips/tricks, DevOps, scale]
---
{% include JB/setup %}

There is a dirty little secret waiting to be discovered by anyone with a great idea: Deployment. Regardless of how amazing your functional prototype is it doesn't amount to anything if it can't be deployed and successfully scaled. 

It's not difficult these days to find some entrepreneurial type who has a great idea and only needs "a programmer to build it for them" (for a flat fee of course) and they'll take it from there. The formula goes something like this:

1. Get some devs to build a prototype
1. Marketing
1. ???
1. Profit!

Most programmers who come across a scheme like this see how doomed it is from the start. Knowing of course, that code requires maintenance, and users require support. 

Developers and engineers can sometimes be guilty of feeling like they're the smartest people in the room. Yet many of these same developers who'll waste no time railing on the stupidity of half brained start-up ideas and fauxtrepreneurs are guilty of an even greater sin on their own ventures: 

### Deployment as an Afterthought

Think about the last project you worked on: when did you start talking about how it should be hosted, how to back it up, how to restore it if it crashed, or how to push out new code? 

For too many projects these conversations and decisions occur as the project is wrapping. The last possible second. A perception exists that digging too far into any one of these areas constitutes "premature optimization" or is "putting the cart before the horse"; other common engineer/developer traps. As someone with experience working on projects both large and small I feel strongly that nothing could be further from the truth.

I'm not advocating for your JS based game being be deployed behind a load balancer in an auto-scaling array, or ensuring your new project's infrastructure should be able to sustain 500+ requests per second before designing a functional UI. That's not what the "always be deploying" mindset it about. In fact I think adopting the ABD™ mindset is a great way to PREVENT premature optimization.

### Always Be Deploying

So what does it mean to Always Be Deploying? It's got nothing to do with continuous integration, short frequent release cycles, or anything like that. It's something that ANYONE regardless of technical ability can do. Simply put it's believing that:

> The deployment process and infrastructure design are living things which deserve a home in day-to-day discussions, and up-to-date documentation. Everyone on the team should understand the process at a high level.

ABD is actively resisting deployment as an afterthought. It's not prescriptive: there are no technical requirements. ABD is merely a call to action for engineers and executives alike to ask, "are we making good architecture decisions right now at this moment?" and "are we setting ourselves up for future success?" throughout the entire project. 

The temptation we all face, and likely the #1 cause of deployment as an afterthought, is falling back on a previous release process or architecture because "it's what we always do" or "what we did on project X". 

Frankly, there's nothing intrinsically wrong with reusing the same architecture and/or procedure over-and-over, especially if it works! There's something wrong with assuming it's appropriate on every project, and not honestly evaluating that as a team until the last possible moment.



