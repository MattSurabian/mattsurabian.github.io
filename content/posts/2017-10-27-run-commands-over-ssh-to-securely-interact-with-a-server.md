+++
date = "2017-10-27T17:00:00+09:00"
title = "Use SSH: Run One Off Commands To Securely Interact With A Server"
description = "How I use SSH to get information about a remote server quickly and securely."
thumbnail = "images/tin-can-phone.jpg"
+++

This post is [part of a series about using SSH more effectively]({{< ref "posts/2017-10-20-youre-using-ssh-wrong.md" >}}). 
SSH is one of those programs that a lot of developers interact with but few seem to take full advantage of. This series 
highlights some of the very powerful features that have saved me a lot of time over the years.

## Passing Commands To SSH

You can run a quick command on a remote system without needing to open a whole persistent terminal session. When the command
is finished running the SSH tunnel will be closed. There are two ways to do this, because SSH is open source software and why not.

Here’s how to quickly check what version of Java is running on a remote system:

```
ssh username@serveraddress “java -version”
```

or 

```
ssh username@serveraddress -C “java -version”
```

_Note the capital C, lowercase c changes the cipher, which isn’t any kind of move you want to do._

Personally I like to use the `-C` variant because just tacking the command at the end without any kind of flag feels dirty; but 
you know, you gotta be yourself. Both styles are correct and do the exact same thing. 

If the command you're running requires elevated privileges (thus requiring a password prompt), or otherwise requires interaction, like editing 
files or browsing them with `less`, you'll need to also pass `-t` to force a tty session (which is just a fancy way to say a session where one is interacting with a machine).

Some examples of how you can use `-C` and cases where you'll also need `-t`:

**Bouncing a service** 

```ssh -t username@serveraddress -C "sudo service restart nginx"```

**Grepping a root owned log file** 

```ssh -t username@serveraddress -C "sudo grep NOAUTH /var/log/nginx/error.log"```

**Grepping some random file you own**

```ssh username@serveraddress -C "grep PATH ~/.bashrc"```

**Reading a log**

```ssh -t username@serveraddress -C "sudo less /var/log/nginx/error.log"```

**Monitoring resource consumption** 

```ssh -t username@serveraddress -C "top"```

**Editing a config file in place**

Please don't do this, consider using configuration management and templating 

```ssh -t username@serveraddress -C "vim ~/.bashrc"```

While it's true that my favorite kind of infrastructure is the kind that *no one is allowed to SSH into* that's not a 
reality for most people. Every now and then we need to login to a remote system and make things happen. Hopefully that's
been made a little easier for you after reading this post.
