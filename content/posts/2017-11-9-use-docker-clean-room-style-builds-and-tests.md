+++
date = "2017-11-09T17:00:00+09:00"
title = "Use Docker: Clean Room Style Build and Test Environments"
description = ""
thumbnail = "images/clean-room.jpg"
+++

This post is [part of a series about using Docker in your development workflow]({{< ref "posts/2017-10-25-how-docker-improves-my-workflow.md" >}}) even if you’re not ready to start shipping containers into production. Whether you’re just getting started with VMs and Vagrant, have avoided Docker because it seems too complicated, or are just looking for ideas on how to use Docker more, this series covers some of the things that helped me start loving it.

## Docker is a consistent on-demand computing environment

My next aha moment with Docker came while I was working with an application that had a lot of prerequisites that developers were expected to set up on their machines in order to produce a build or run the test suite.

Subtle differences between all of our machines meant that few or none of us were running the exact same thing that our continuous integration machine was shipping to production. We also dealt with our fair share of false alarm test failures because something would change in one of our environments as a result of one of the other projects we were also working on. These challenges forced us to move slower and occasionally would result in us releasing buggy software.

Then I remembered that Docker could do more than just run daemonized dependencies in the background. It could also do arbitrary computing in a totally isolated environment. I altered our make file to run our build and test commands inside of a Docker container running the same Linux flavor our CI was using and viola. Everyone suddenly had access to the exact same build artifact and the exact same testing environment our production pipeline was using. Best of all the chore of setting up our local build and testing environments was gone.

Unfortunately there’s no magic one liner for this, but I’ll definitely devote future blog posts to digging into this more as it comes up for me again. 

## There are some good tutorials and GitHub repos out there

**JavaScript**

  - [Caleb Sotelo's Ultimate front end and node JS dev setup.](http://paislee.io/the-ultimate-nodejs-development-setup-with-docker/)

**Go**

  - [An example of an amazing containerized build pipeline from the Kubernetes team.](https://github.com/thockin/go-build-template)

**.NET**

  - [Microsoft's guide to using .NET and Docker with some example projects.](https://blogs.msdn.microsoft.com/dotnet/2017/05/25/using-net-and-docker-together/)

**PHP**

  - [An NGINX based PHP and Docker workflow from Chris Tankersley](http://ctankersley.com/2016/07/27/my-php-docker-workflow/)
  - [An Apache based PHP and Docker workflow from Rachel Andrew](https://perchrunway.com/blog/2017-01-19-getting-started-with-docker-for-local-development)

## Things to keep in mind when using Docker to wrap builds and tests:

  - Shared volumes are great, but tend to be a little finicky if you’re not using Linux. Definitely try with shared volumes first but if folks on your team start having trouble just use the copy command inside a Docker or Docker Compose file. It’s not worth the time suck of chasing random userid and permissions issues.
  - I love using the Docker CLI directly and learning about all the tool's flags, but most people aren’t going to want to deal with all that every time they need to run tests. Help folks out by writing a script or integrating into an existing Makefile. That way people still just run `make test`, `npm test`, `make build`, `npm run build`, etc.
  - Avoid doing anything that would prevent folks from being able to run tests or builds OUTSIDE of the Docker environment. While the ideal workflow should revolve around Docker and some task runner that shouldn’t be at the expense of the traditional toolchain. Ideally you want to be running your regular build or test commands, just inside a consistent environment. For example: if your build or test runner needs environment variables specified, don’t set them in the Dockerfile, write a wrapper script that specifies the variables so it can be run inside or outside of the container.

Hopefully this helps convince you to try using Docker in your development workflow even if you’re not ready or 
interested in shipping containers to production. Checkout the [previous post in this series about using Docker for 
isolated ad-hoc dependencies]({{< ref "posts/2017-10-30-use-docker-isolated-ad-hoc-dependencies.md">}}). The next post is about how to use Docker as a personal network lab.
