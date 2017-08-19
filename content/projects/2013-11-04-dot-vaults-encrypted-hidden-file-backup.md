+++
date = "2013-11-04T17:00:00+09:00"
title = "Dot Vaults: Encrypted hidden file backup"
description = "An outline of my dot-files project and how it can help to backup hidden configuration files. Update: Probably don't use this. 3DES isn't so secure these days."
thumbnail = "images/vault.jpg"
+++

*This post is here for historical reasons. Don't use this. 3DES via openssl is not considered secure.*

Backing Up Dotfiles
-----------------------------
After hearing about it for months on [Security Now](http://twit.tv/show/security-now/), I finally decided to give [Carbonite](http://carbonite.com) a try. I'd been trying to decide between [CrashPlan](http://crashplan.com) and Carbonite for far too long and the fact that Carbonite supports a pod cast I enjoy finally put me over the edge. 

Carbonite, like many other automated backup solutions, doesn't provide support for dotfiles. Dropbox and Github were options; I took a good long look at [Ben Alman's dotfiles project](https://github.com/cowboy/dotfiles) and a few others, but found they focused more on dotfile management and new machine bootstrapping and didn't offer much in the way of securing sensitive content. 


[Dot Vault](https://github.com/MattSurabian/dot-vault) is Born
------------------
All I wanted was a secure storage method that would allow me to turn all of my dotfiles into one single encrypted file that I could backup, sync, or share anyway I chose without needing to first clear it of sensitive content. My requirements were simple:

 - I tell the tool where the dotfiles are, rather than bring the dotfiles to the tool.
 - The tool outputs a single secure vault file
 - The tool can import a vault and restore the files it contains, without needing the config file that generated it.
 - The tool's commands can be automated using cron to ensure my dotfile vault is always up to date. 

#### You Get the Dotfiles

Most of my dotfiles are in my home directory, but sometimes they're elsewhere, and I didn't want that to matter. The straightforward new-line delimited path file ala .gitignore is a format I'm fond of, so that's how the configuration .dotvault file is formated.

#### Encrypted Output

I don't like having to keep one edited version of a file with sensitive information removed just for backup purposes. I want to be able to backup the file, exactly as it sits, exactly where it sits, without having to do a thing. A single encrypted file as output seemed the only way to accomplish this

#### Self Restoring Import

I liked the idea of an encrypted file that knew where all of its content was supposed to go and put it there. I opted for a string replacement routine to keep the tar file from containing several "placeholder" directories only responsible only for preserving the integrity of longer paths.

#### Automated

I wanted to be able to setup a cron job to ensure that my backup was always getting the latest data.


How It Works
------------
[Dot Vault](https://github.com/MattSurabian/dot-vault) has an extensive readme all about how to use it and how vaults are created. 

Other Applications
------------------
Now that I'm on the other side of this project I guess there's nothing to say that you couldn't use dot vault to create all kinds of aggregate, secure, "self restoring" backup files. I suppose I'll have to extend it to accept a dotvault configuration file via the command line in the next release.