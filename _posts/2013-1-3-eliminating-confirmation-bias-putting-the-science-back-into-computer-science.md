---
layout: post
title: "Eliminating Confirmation Bias: Putting the Science Back Into Computer Science"
description: "Confirmation bias is the tendency to favor information that supports one’s views or hypothesis. Every time you’re troubleshooting a problem and think, “I know it can’t be ________” you might be guilty of it. As a web developer I know that feeling all too well. Think about your last debugging experience..."
category: Computer Science
tags: [debugging, computer science, scientific method]
---
{% include JB/setup %}

Confirmation bias is the tendency to favor information that supports one’s views or hypothesis. Every time you’re troubleshooting a problem and think, “I know it can’t be ________” you might be guilty of it. As a web developer I know that feeling all too well.

Think about your last debugging experience. Did you solve the problem as quickly as you expected? How familiar with the codebase were you?  [Workplace studies](http://www.codinghorror.com/blog/2006/09/when-understanding-means-rewriting.html) have shown that the majority of a programmer’s time is spent understanding, rather than producing code. And that stands to reason, as it often takes a deep understanding of a language or codebase to produce a minimal amount of elegant, functional code. Unfortunately, it seems that the more familiar one is with a given problem the greater the tendency toward bias becomes.

<br/>
>_It ain’t what you don’t know that gets you into trouble.<br/>
>It’s what you know for sure that just ain’t so._ <br/>-- <cite>Mark Twain</cite>
<br/>

You Are A Scientist
--------------------
It is important to proceed logically towards a resolution. Too often, inexperienced developers end up going off on something completely tangental to the problem at hand . If I find myself having more than one, “oh it must be ____” moment when dealing with a problem I step back and do a quick self-check for confirmation bias. When I find myself going off on a programming tangent it’s usually because I’ve picked my conclusion before properly assessing the problem.

The first step of my debugging process is to define the parameters necessary to replicate the problem. Some problems are painfully intermittent (I call this chasing ghosts) and it can seem impossible to nail down a pattern; take solace in the fact that computers are machines that do what we tell them… most of the time. Often these problems seem impossible to nail down because we don’t have access to all the necessary information. When dealing with these situations it becomes even more important to build on reproducible observations and not theories or feelings.

Don’t get me wrong, there are plenty of times when our feelings and instincts are correct, and I’m not suggesting that they should always be completely disregarded- but unless the problem is properly investigated and defined you can never really trust in your proposed solution. Sometimes a seemingly small bug is the effect of a much deeper problem, and the sooner those situations can be identified the better.

 <br/>

Deceptive Error Messages: Information Bias
--------------------
Sometimes confirmation bias creeps in despite our best efforts to prevent it. Error messages, exceptions, and log data are critical to tracking down bugs, but blindly following them can lead to a deep, dark hole.

Some time ago I was helping deploy a single page web app on a client’s server. The app had gone through extensive testing in a development environment but this would be the first time it was deployed to production. On deployment, chrome’s web inspector was reporting 404 errors on two files in the api directory. A quick look at the project files showed there was no API directory. The fix seemed clear, track down those missing files. And so began the chain of events that accompanies such projects: emails and calls trying to find out why necessary files hadn’t been delivered with the project.

In reality the files weren’t missing at all, they never existed in the first place. The problem was one of routing. The htaccess file was missing a RewriteBase parameter and API calls were being routed to the wrong place, causing the 404. By taking the time to assess the full situation and read over the code we were able to avoid a lot of wasted time and misplaced blame.

 <br/>
Looking In The Wrong Places: Misdirection Bias
--------------------
My favorite games to play in a pool are the throw, dive, and retrieve variety. They’re a lot of fun and a pretty good workout too. While working on this article I had occasion to frolic around in a pool playing just such a game with my hotel room key. It’s a simple game: throw the card, swim to where it landed, dive down and find it. I was playing the game with another developer and we had just finished a conversation about confirmation bias. I threw the card across to the deep end of the pool and we started the search. Ten minutes later we still hadn’t found the card. We had been scouring the bottom of the pool and both were pretty confident in the location we saw it land, but still we were coming up empty. Surface tension is a funny thing. Most of the time the card readily sinks, but sometimes it lands just right and sits on top of the water, almost invisible at a distance, free to drift around with the current created by two idiots diving around near it. We were so used to diving to the bottom of the pool, it hadn’t even occurred to us that the card could be sitting right on top of the water!

<br/>
Conclusion
--------------------
If there is one thing I’m certain of it’s that we can’t escape confirmation bias for very long- it’s human nature to favor information that reinforces our existing beliefs. Unfortunately, as developers we don’t have the luxury of human nature; all we have is science.