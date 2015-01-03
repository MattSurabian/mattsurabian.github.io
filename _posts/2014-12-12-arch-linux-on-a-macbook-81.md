---
layout: post
title: "Arch Linux on a Macbook 8.1"
description: "My experience running Arch Linux on a Macbook Pro 8,1."
category: tinkering
tags: [tinkering, devices, tips/tricks, Linux, Arch, Apple, OS X, Yosemite]
---
{% include JB/setup %}

## Pass The Tinfoil

I grew up using Apple computers, and even though my chosen career path has thoroughly exposed me to Windows and Linux, my daily driver has always been some form of mac. But for the last several years as their mobile devices have taken over; I've become disenchanted. 

Personally, I don't much care for the cellphone or tablet form factor and am not a user of iOS. As a result I feel I'm drifting further and further from Apple's target market.

 - I don't want a unified interface between my mobile device and my laptop. 
 - I don't want my hardware and OS manufacturer to also be my: backup provider, software gate keeper, and message relayer.
 - I don't want all my component hardware to be soldered to the mother board, regardless of how thin that makes the overall machine. 

When I think about things like [Yosemite discretely sending user information and data to Apple's servers](https://github.com/fix-macosx/yosemite-phone-home), or how iMessage is touted as secure person to person communication [despite Apple managing all the public keys and message transmission](http://www.macworld.com/article/2055640/researchers-challenge-apples-claim-of-unbreakable-imessage-encryption.html), I get disheartened with the future of the operating system and wonder why I still use it at all. 

*This isn't Apple's problem, it's my problem.* There are millions of people who love what OS X and iOS offer them and I don't want or expect Apple to change on account of my grumpy attitude. For a long time I just resigned myself to that.

The tweet that finally broke the camel's back was a link to a story about how [Yosemite sends documents not stored in iCloud to iCloud anyway, completely unencrypted](http://www.washingtonpost.com/blogs/the-switch/wp/2014/10/30/how-one-mans-private-files-ended-up-on-apples-icloud-without-his-consent/). The use case makes sense: maybe a user wants to resume editing the document on their iPad or iPhone; but for me it was just too much. I ran to the kitchen and grabbed some tinfoil to make myself a hat, then dug out the old 120GB solid state drive I had kicking around. It was finally happening, I was switching to Linux.

## Why Arch Linux?
As a deployment fiend and ex-sys admin I'm very comfortable at the command line, so the thought of having to build up the entire system from scratch didn't really intimidate me. Arch's wiki is extensive and I knew friends and colleagues that were already using Arch who could answer questions and help me troubleshoot random issues when they came up. Plus, I had dabbled in using Ubuntu and other Debian based systems as my daily driver in the past and it never really stuck; which is to say nothing of the spotty support of my MacBook's hardware in that ecosystem.

## How Does It Even Work!?
This post (clearly) is not meant to be a step by step guide on how to install Arch on a Macbook Pro, for the express reason that I wasted so much time fiddling around with other people's random and outdated guides that I don't think it would really be helpful. Instead I want to offer some tips and tricks that saw me through the darkest times of the system build in hopes that this post motivates and aides someone else in seeing their own switch through to completion.

### Avoid Step By Step Guides That Aren't the Arch Wiki's Beginner Guide
The [beginner's guide](https://wiki.archlinux.org/index.php/beginners%27_guide) and the wiki pages it links to is really all you want to look at. The answer to almost any question you're going to have is in there somewhere. **Remember that it's not meant to be followed completely from top to bottom! Pay special attention to the heading of the sections as you scroll through.** Macbook Pro's are UEFI systems, so completely skip the BIOS based bootloader section, for example.

### Use a Simple Partition Scheme
If you're not a power user and don't really understand partitioning, skip LVM completely, skip distinct `/home` and `/root` partitions, and if you've got more than 4GB of RAM skip `/swap` too. Just create a ~200MB `boot` partition with the type `ef00` (EFI System) formatted in FAT `mkfs.fat -F32 /dev/sda1` and a `root` partition with the rest of the space of type `8300` formatted with `ext4` `mkfs.ext4 /dev/sda2`. This is the absolute minimum partition scheme required and it'll probably work just fine for you.

### Full Disk Encryption
This is something I *really* wanted to do but didn't because after several false starts I found myself just wanting my machine to boot. Creating the encrypted root partition was straightforward, as was decrypting and mounting it manually; but I got really confused at the bootloader stage, and the additional complexity of handling the decryption in the bootloader config was a challenge that arrived at the wrong time. The bad thing about this decision is it means eventually I'll need to completely redo the setup of my root partition as there isn't a way to go back and encrypt it after the fact.

### Bootloaders
I went into this thinking I understood computers and how they work. This stage of the process really challenged that assumption. While I understand what a bootloader does in theory, the nuances of the various configuration options available let alone *choosing* a bootloader was very overwhelming to me. I found myself in a horrible Google loop, reading guide after guide, parsing random people's configuration files that promised to work great on a Macbook. 

After my fifth failure at this stage I took a step back and decided that anchoring the research around my specific Macbook 8,1 model was the problem. I opened the [Beginner's Guide on the Arch wiki](https://wiki.archlinux.org/index.php/beginners%27_guide) and nothing else. I carefully followed it and when something didn't make sense I read it again and again until it did. This decision was so critical to my success. In the end I installed `Gummiboot` exactly as the wiki suggested and everything worked perfectly. 

### Holy $h*t it Boots
This was an amazing feeling, but I wasn't out of the woods yet. I had no wifi, no window manager, no desktop environment, no sound, no multimedia keys, no printing. It was stark. But it booted up and my ethernet jack was working. It's definitely worth basking in this moment when you get there; so I did.

### Drivers, Programs, and the AUR
In order to really make Arch Linux sing you're going to want to get hooked up with packages that live inside the AUR and that means manual dependency hunting, compiling, configuring, and installing. HA! Just kidding, follow [this quick guide to install `yaourt` via pacman](https://archlinux.fr/yaourt-en) and watch in awe as other people's brilliant hard work pays off for you! Open source for the win. I can't thank my friend [Tim Branyen](https://twitter.com/tbranyen) enough for [telling me about `yaourt` the second I tweeted about switching over to Arch](https://twitter.com/tbranyen/status/541268065431076864). It really saved my time and sanity.

**Essential Packages For The Macbook 8,1**

 * `lib32-alsa-lib` (sound)
 * `alsa-utils` (sound)
 * `broadcom-wl` (wifi drivers, in AUR use yaourt. I found this more stable than `b43-firmware`)
 * `networkmanager` (for networking, make sure to stop any netctl services before starting NetworkManager)
 * `xf86-input-synaptics` (touchpad driver, in AUR use yaourt)

**Packages You're Probably Going To Want**

 * `lib32-apulse` (sound for Skype, see the [Arch wiki article on Skype](https://wiki.archlinux.org/index.php/skype))
 * `slock` (to lock the screen)
 * `openvpn` (vpn connections)
 * `openssh` (ssh)
 * `python-pyenchant` (for spell checking)
 * `aspell-en` (for English spell checking)

### Printing
Check the wiki for info about printing and [support for your printers of choice](https://wiki.archlinux.org/index.php/Category:Printers)

### Desktop Manager
So many choices; I picked `lxdm`. The config file has lots of cool options and lives, as you may have guessed in `/etc/lxdm/lxdm.conf`. I use the `ArchStripes` theme.

### Window Manager
I went with `xfce4` because it's simplistic and I dig that. I also installed `xfce4-goodies`. For wifi status in the notification window I installed `network-manager-applet`. I get screen shot functionality from `xfce4-screenshooter` which is included in `xfce4-goodies`. I added a keyboard shortcut to the region command `xfce4-screenshooter -r -s /home/USERNAME/Desktop/` to make taking screenshots easy.

### Browsers and DRM Flash Content
If you want to watch Netflix, Amazon, Youtube or other DRM'd sources of content you'll find that you can't without doing some work. I installed `hal`, `hal-info`, and `flashplugin` to get this content to work in Firefox. Chromium was another story. I installed `chromium` and `chromium-pepper-flash` but it didn't matter. DRM flash still couldn't be played. Fortunately some friends of mine reminded me that `google-chrome` existed in the `AUR` and it "just worked". Did I mention how much I love `yaourt`?

### Doomsday Prepping
Now that you're a Linux user it's probably a good idea to keep a small bootable flash drive with you which you can use to boot a stable version of Arch and `arch-chroot` into your main partition. This will allow you to recover your system in the event a package conflicts or a configuration file gets corrupted while you're playing around with settings/drivers. The genius bar isn't going to be much help to you anymore.

## When The Smoke Cleared
Once I got everything setup, using the system was really natural. I'm wrapping up my first week using Arch and other than the slightly "boxy" feeling of the UI (which is `xfce4`'s fault) I've got nothing negative to say. I do wish the wifi driver was better as I took a hit in signal reception, but that's not Arch's fault. My system is stable, performant, and still encased in aluminum. If you're thinking about doing this I'd suggest making sure you're comfortable on the command line first, and that all your day to day programs are either supported or have an alternative available that you can live with. This whole process took me about a day and a half. I then spent a week tweaking the system to my liking, which is a process that has been kind of fun, honestly. 

I'd recommend starting with a fresh hard drive instead of toying with backup and restore in the event you decide you want to switch back. Having the security of a functional OS X drive to plug back in if I got over my head or decided I didn't really like using Arch made this whole experience significantly less stressful. 

Best of luck!
