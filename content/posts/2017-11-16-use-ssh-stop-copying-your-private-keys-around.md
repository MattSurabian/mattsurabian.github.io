+++
date = "2017-11-T17:00:00+09:00"
title = "Use SSH: Stop Copying Your Private Keys Around"
description = "How to use SSH agent to maintain secure access to everything while connected to a remote machine."
thumbnail = "images/keys-in-door.jpg"
+++

This post is [part of a series about using SSH more effectively]({{< ref "posts/2017-10-20-youre-using-ssh-wrong.md" >}}). 
SSH is one of those programs that a lot of developers interact with but few seem to take full advantage of. This series 
highlights some of the very powerful features that have saved me a lot of time over the years.

## The most common pain point is git

A common problem I see folks runing into is when they try to check out code from git repositories on remote systems. 

Locally you can just run a regular old SSH clone like: 
```git clone git@github.com:MattSurabian/DuckHunt-JS.git``` 

and the download starts immediately if the git server has been configured to know about your machine’s public key. 

On a remote system though, your private key doesn’t exist and the same command will throw an access denied error and 
you’ll likely need to fall back to cloning over `https` [like some kind of username and password typing animal]({{< ref "posts/2017-10-20-automatically-configure-ssh-key-access.md" >}}) or worse:
using `scp` to shuffle giant git repo directories around. Yikes.

Of course, it’s not just `git clone` that will cause you to have to deal with this problem, but that’s definitely the most common 
reason I’ve seen folks encounter it. Really, this will happen anytime a process is making additional remote connections and 
expects to authenticate with a key pair that the remote machine doesn’t have. 

## Do you leave your keys in your door?

To get around this, a lot of people copy their private keys around to anyplace they need them. Not only does this not
scale if you’re someone that has more than one or two SSH keys for things, but from a security perspective it's like 
leaving your keys in the door to your office. It's easy to loose track of where your keys are all living and it's even
harder to ensure the security of the remote machines over time- especially if other folks have access to the same box and
could potentially copy your private keys themselves. Fortunately, your operating system is ready to help you with a built 
in program called `ssh-agent`. 

To take advantage of SSH Agent you need to tell it about your keys. If you only have a single default SSH key you can 
just run `ssh-add` (on OS X run `ssh-add -K`) if you have multiple keys use `ssh-add PATH/TO/PRIVATE_KEY`. This stores 
your key data in your local SSH Agent which can be made to handle cryptographic challenges from remote systems using a 
feature called SSH Agent forwarding.

Note that generally SSH Agent will only hold onto these keys until you reboot your system. If you need to remote into systems frequently 
you should consider updating your local `~/.bash_profile` file by adding the `ssh-add` commands for the keys you regularly use 
so that your agent is always ready to go. Some operating systems will do this for you automatically once you've used SSH Agent once, 
but I always add the lines in my shell profile just to be sure, and because I’m a crazy person that uses Linux.

Now the next time you SSH into a machine just pass the `-A` flag which enables the SSH Agent forwarding feature: 

```
ssh -A username@serveraddress
```

This feature ensures that whenever the remote system receives a cryptographic challenge requiring a keypair it doesn’t have, 
the request is sent over your SSH tunnel so your local machine can respond. That's important to understand; SSH Agent is not just
copying your keys onto the remote system for you, instead your keys remain securely on your local system and auth
requests are simply forwarded along.

The end result is that your remote machine is granted access to anything your local machine has access to. The best 
part is that SSH Agent will loop through every key it knows about until it finds one that works or it runs out of keys. 
So if you have lots of keys you don’t need to explicitly say which to use for any given task, so long as they’ve been `ssh-add`’d your 
agent can handle the rest.

## A PSA about encrypting your private key

I give this speech all the time to folks new to SSH and I'm sure it mostly falls on deaf ears, but please understand:
*possession of your private key is proof of identity*. That means if someone were to steal your private key, say from a 
remote system you copied it onto, that thumb drive you carry around, your unencrypted laptop, etc they could effectively 
impersonate you to any service or system setup for key based auth. 

SSH has built in protection from this threat model: it allows you to easily add a passphrase to your key. That
passphrase encrypts your key so it cannot be used until the passphease is entered and the key is decrypted. Please set a
passphrase on your SSH keys today! 

SSH agent and your OS's keychain are able to save these passwords so you don't have to
constantly enter it everytime you use your key (only once per key per session). While it's true that the majority of
folks will probably never have their private key stolen, setting a passphrase and using SSH Agent forwarding 
improves your SSH security dramatically, while also making it a lot easier to work securely on remote systems as this post
has hopefully shown.

If you want to learn more about how to leverage SSH's features checkout the previous post in this series about [running 
ad-hoc commands to securely interact with remove services]({{< ref "posts/2017-10-27-run-commands-over-ssh-to-securely-interact-with-a-server.md" >}}).