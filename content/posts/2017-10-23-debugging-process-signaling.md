+++
date = "2017-10-23T17:00:00+09:00"
title = "Debugging Process Signaling In Bash"
description = "How I used a short bash script that trapped signals to figure out why a program wasn't exiting when hitting ctrl-c."
thumbnail = "images/signal-plane.jpg"
+++

Recently I was working on this blog in my favorite IDE: [IntelliJ IDEA](https://www.jetbrains.com/idea/). This blog is a [hugo](http://gohugo.io/) site and recently I noticed
that whenever I ran `hugo serve` locally from inside my IDE I was unable to `ctrl-c` out of it. I didn't remember this 
being a problem the last time I was using hugo. So I did what any developer might do in this situation: cried to the internet for help.

My first stop was GitHub issues, and as if by magic I found an issue called "[hugo server keeps running after CTRL+C](https://github.com/gohugoio/hugo/issues/3902)" that looked promising;
however, the issue was occurring only in Windows on "Bash on Windows" and no one else was able to reproduce it. It definitely sounded like
a problem with the environment and not hugo itself. So I went back to poke at my situation some more and sure enough, when 
running hugo from my real terminal client everything worked as expected. There was definitely something going wrong with the embeded terminal
in IDEA.

Being curious, I decided to dig a little deeper and figure out what was going on. I was definitely able to kill the process with a `SIGINT`
which is what I expected pressing `ctrl-c` would send:

```
kill -SIGINT $(pgrep hugo)
```

Seeing this I figured the shell inside IDEA must be sending something other than `SIGINT`, but what? I wrote a quick shell script to help me figure
it out:

```
#!/bin/bash

SIGNALS=$(kill -l | grep -w -o "\S\+" | grep -v ")" | tr '\n' ' ')

setTraps() {
    shift
    for sig ; do
        trap "echo $sig" "$sig"
    done
}

setTraps $SIGNALS
read
```

This script will run but block waiting for user input. Whenever the process receives a signal it will output the signal name
to the command line. This script must be force killed with `SIGKILL` or modified to exit after every trap.

In the end, it turned out that this is a [resurrected bug in IDEA's embeded terminal](https://youtrack.jetbrains.com/issue/IDEA-168006) and that when `ctrl-c` is pressed _no signal_ is sent.

Bummer for me, but maybe this script will be useful for someone else.

<br> 


