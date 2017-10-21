+++
date = "2017-10-20T18:00:00+09:00"
title = "Use SSH: Configure Key Based Access Automatically"
description = "Stop typing your password every time you SSH to a remote server. Use this one line script to automatically setup key-based authentication."
thumbnail = "images/enter-password.jpg"
+++

This post is [part of a series about using SSH more effectively]({{< ref "posts/2017-10-20-youre-using-ssh-wrong.md" >}}). 
SSH is one of those programs that a lot of developers interact with but few seem to take full advantage of. This series 
highlights some of the very powerful features that have saved me a lot of time over the years.

## Stop typing your password when connecting to remote servers
I _really_ hate having to type my password over and over again when logging into remote machines. If you don’t have 
something already automatically giving you SSH access to the servers you access regularly don’t worry, you can 
give that access to yourself using the `ssh-copy-id` command. This should work out of the box on OS X and Linux; if 
you’re on windows you can use something like [this gist](https://gist.github.com/ceilfors/fb6908dc8ac96e8fc983) to mimic it.

`ssh-copy-id username@serveraddress`

If you want to add a key other than your default one you can do that with `-i`:

`ssh-copy-id -i ~/.ssh/mykey user@serveraddress`

Now the next time you run `ssh username@serveraddress` you should be immediately logged in without being prompted for a 
password.


