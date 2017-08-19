+++
date = "2015-06-02T17:00:00+09:00"
title = "Podcasting With Jekyll"
description = "My experience using Jekyll to power the Rubber Ducking Podcast website, and an overview on how to host a static site using S3."
thumbnail = "images/jekyll-logo.png"
+++

At the beginning of this year Matt Gagne, Adam Jarret, and I finally decided to follow through with our idea to record a podcast together; it's called [Rubber Ducking](http://rubberduckcast.com/) and we try to record twice a month. We started out by making sure everyone was setup to properly record vocals and did a few riff sessions to determine the overall tone and content. Given that we're three loudmouths from New England, this was the easy part. Deciding on how we wanted to record it, host the site, and serve the audio files on the other hand...

## Recording

#### Audio Gear

The first thing we all did was get the same microphone. We opted for the [USB version of the blue snowball mic](http://www.amazon.com/gp/product/B002OO18NS/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B002OO18NS&linkCode=as2&tag=rubberduckin-20&linkId=2H4SNRRDFEQJKDKY) because it was reasonably cheap and good quality. But all that really mattered was that our input method was consistent.

#### VOIP

Because it's a ridiculous resource hog, seemingly worse with every update, and [houses a backdoor for the NSA](http://www.theguardian.com/world/2013/jul/11/microsoft-nsa-collaboration-user-data), we weren't thrilled with the idea of using Skype. We opted to use Google Hangouts instead, which admittedly is only mildly better with resources and security. Ultimately though, we could have these conversations using any means of audio communication since we record "double-ender" style.

#### Double-Ender Style

To record a double-ender, each individual records only their own voice (bonus side-effect: this forces everyone to wear headphones) and the tracks are combined during the editing phase. We all get on a call, start a local recording, sit silently for a moment, and clap once at a predetermined time as reflected by [time.gov](http://time.gov). This makes it easy to sync up the tracks later regardless of signal latency issues. 

We record in this style because it allows us to get high quality audio without worrying about connection issues; if someone's connection drops, everyone leaves their recording running and when the person rejoins we pickup where we left off. The chunk of silence can be easily removed while editing without needing to re-sync the tracks. Recording double-ender style also means we don't need to rely on random recording add-ons/plugins for Hangouts or Skype. 

#### Sharing Our Voice Recordings

We use [BitTorrent Sync](https://www.getsync.com/) to share our individually recorded voice tracks with each other, but are considering switching over to [Syncthing](https://syncthing.net/) because it's open source. Either way, the feature-set is the same: both provide a secure, free, convenient, and decentralized way to share large files between multiple computers. [DropBox](https://www.dropbox.com/personal) could be used just as easily, but file size could cause an issue for people without a premium account.

## Editing

Simple, convenient, open source, and free- [Audacity](http://web.audacityteam.org/) works great to both record individual voices and edit the podcast together. At minimum, the sync-up claps need to be aligned, which is straightforward since it shows up as a big spike in the waveform. We also cut content for length and use filters to reduce background noise.

## Hosting

An auto scaling server setup is cool, but expensive and excessive for a small podcast website. We didn't want to sacrifice scalability or stability, so we decided to use Amazon S3 and produce a static site that could be easily cached. Hosting for our modest listener base (around 900 visitors to the site this month) is typically under $1 per month.

#### Static Files

There are a number of static site generators available but we opted for [Jekyll](http://jekyllrb.com/), since two-thirds of us were already comfortable using it. The code for our [podcast's website](https://rubberduckcast.com) lives on [github](https://github.com/rubberduckcast/rubberduckcast.com), and the workflow for pushing the site live relies heavily on [Grunt](http://gruntjs.com/) (more on this later). 

#### Why Bother?

Fair question. There are lots of options for hosting a podcast site. The most straightforward one, which Matt Gagne threatened me with, was a [SquareSpace](http://www.squarespace.com/) site and a [feedburner account](http://feedburner.com/). Honestly, that's a fine solution and is employed by many a successful podcast. 

We opted for Jekyll and S3 because it was the cheapest option (especially at scale) and didn't require us to give up control over the site or feed. If you're a moderately technical person and thinking about starting a podcast and hosting the site yourself, our hope is that something in here motivates you to make that happen.

#### Episode Hosting

For maximum flexibility, we host our episodes in a different S3 bucket from the site and serve them on the subdomain: `episodes.rubberduckcast.com`. This allows us to [track episode downloads](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/shell.js#L15-L16) using [S3 logs](https://github.com/wvanbergen/request-log-analyzer#request-log-analyzer-) without website access noise. It also allows us to have a [simple deployment script](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/shell.js#L9) and keep large binary assets out of the site repository.

## Site Design

We're not designers, but we tried to make an attempt by using [Colour Lovers](http://www.colourlovers.com/palettes) to find a reasonable color pallet and [ShutterStock to find vector images of ducks](http://www.shutterstock.com/cat.mhtml?autocomplete_id=143321356231711920000&language=en&lang=en&search_source=&safesearch=1&version=llv1&searchterm=ducks&media_type=vectors). 

We then decided on a rudimentary plan to make the site responsive, [wired up](https://github.com/RubberDucking/rubberduckcast.com/blob/master/_includes/header.html#L3-L6) the [picture element](https://responsiveimages.org/) for our logo, and [set](https://github.com/RubberDucking/rubberduckcast.com/blob/master/styles/layouts/default.styl#L5-L10) [some](https://github.com/RubberDucking/rubberduckcast.com/blob/master/styles/layouts/default.styl#L35-L39) [break points](https://github.com/RubberDucking/rubberduckcast.com/blob/master/styles/layouts/default.styl#L68-L74) based on what looked best to us given the content we had.

We use [stylus](https://learnboost.github.io/stylus/) and the [grunt stylus plugin](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/stylus.js) to [manage our CSS](https://github.com/RubberDucking/rubberduckcast.com/blob/master/styles/index.styl). During [active development](https://github.com/RubberDucking/rubberduckcast.com/blob/master/styles/layouts/default.styl#L35-L39) a [watch task](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/watch.js) recompiles CSS for us. We use the [grunt-concurrent plugin](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/concurrent.js) to allow us to watch for both changes to our Jekyll code and stylus modifications.

## The Workflow

After a new episode is recorded, we give it a title and write up both a long and short description which are added to the audio file's tags during the export process. Matt Gagne is our fearless editor, and when draft episodes are ready for our review he puts them in our BitTorrentSync folder. When everyone has given a listen and a :+1: Matt moves the episode draft into a final version folder which is symlinked into the Jekyll project directory as `_audio`. This directory is ignored by git and only symlinked into the project to provide a unified workspace. 

Our episodes are output as `m4a` but `ogg` versions need to be created for [cross browser streaming support](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats#Browser_compatibility). We automate this with a Grunt task we've named `oggify`. The [code for this task](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/ffmpeg.js) looks through the `_audio` folder for all files that have an `m4a` version but not an `ogg` version, and uses [ffmpeg](https://www.ffmpeg.org/) to do the conversion on the command line. This means ffmpeg must be installed on your machine before running the task. The newly created files are then pushed up to our `episodes.rubberduckcast.com` S3 bucket using the `push-episodes` [Grunt task](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/shell.js#L12).

At this point, even though the new episode exists and is available online, it's not in the feed or showing on the site. For that to happen a new markdown file is created in the `episodes/_posts` [directory](https://github.com/RubberDucking/rubberduckcast.com/tree/master/episodes/_posts) which looks something like this:

<script src="https://gist.github.com/MattSurabian/a5140257def6f699a8ab.js"></script>

The actual file size and playback length are added, along with the content written earlier. The Grunt `dev` task is then used to ensure everything looks as it should before pushing the built site live using the `deploy-prod` task. The post's markdown file has all the information necessary for the front end of the site and [the XML based feed](https://github.com/RubberDucking/rubberduckcast.com/blob/master/_includes/feed.xml) [required by the Apple store](https://www.apple.com/itunes/podcasts/specs.html) and supported by many podcatcher applications.

Managing the site in this way makes it approachable and efficient for any of us to publish a new episode. No database or manual XML editing required, and the data that drives the site is always in sync with the feed.

## The Prerequisites

While it's true that once setup this podcast hosting method is pretty low on cost and maintenance; the initial setup is non-trivial. Honestly, these prerequisites could probably be a series of posts about static site hosting, but instead I'm opting for this high level crash course. There are lots of resources out there for learning this stuff, hopefully what's provided here establishes a good starting point and gets you through any confusing portions.

#### A Domain Name

My registrar of choice is [namecheap](https://namecheap.com) but this really doesn't matter; secure a domain name you love from a registrar you trust.

#### AWS

Create an AWS account if you don't already have one. Login to the [management console](https://console.aws.amazon.com) and create some [S3 buckets](https://console.aws.amazon.com/s3/home)! 

  - YOURDOMAINNAME
    - Static site hosting will need to be enabled
  - WWW.YOURDOMAINNAME
    - This will also have static site hosting enabled but will redirect to the YOURDOMAINNAME bucket
  - EPISODES.YOURDOMAINNAME
    - Static site hosting does NOT need to be enabled, but we will use a public-read access control value when uploading episodes to this bucket. This allows public individual episode access but not entire bucket access.

Next, create as many users as you need in the [IAM section of the AWS management console](https://console.aws.amazon.com/iam/home). Keep a copy of the `access-key` and `secret-key`. Also create [a group](https://console.aws.amazon.com/iam/home#groups) for these new podcast users to belong to. The group IAM policy we use looks like this:

<script src="https://gist.github.com/MattSurabian/577ea7094bf8eb812ba1.js"></script>

To use this policy, replace the S3 bucket names with the one's you've created and add it to the new users or the group they all belong to.

#### AWS-CLI

Install the AWS CLI onto your machine (Google for instructions based on your OS, there are lots of options) and create a credentials file in `~/.aws/credentials`. It's a good idea to namespace the credentials using profiles (shown below) to prevent conflicts if you need to access different AWS accounts or resources.

This is what my credentials file looks like. It includes a profile called `rubberduck`.

<script src="https://gist.github.com/MattSurabian/23c31a2f8d987f50b3ba.js"></script>

You can see this profile is used by the [shell tasks which call the AWS CLI](https://github.com/RubberDucking/rubberduckcast.com/blob/master/build/tasks/shell.js#L9).

While it's true that there is a node SDK that we could use, I prefer the standard CLI.

#### DNS

I use [Cloudflare](https://www.cloudflare.com/) for DNS because it saves us some bandwidth, costs nothing, and provides a great management interface. Plus I prefer not to couple my registrar with my DNS provider. Regardless of what you're using, the DNS provider needs to support using only CNAME records for a domain. 

However you manage DNS, copy the endpoint provided in the static site hosting properties panel of your newly created S3 bucket and make a CNAME record for the root domain, the www subdomain, and the episodes subdomain buckets; ours looks something like this:

**YOURDOMAIN Bucket Properties**

<a href="http://i.imgur.com/ysZiFnI.png">
  <img src="http://imgur.com/ysZiFnIl.png" />
</a>

**WWW.YOURDOMAIN Bucket Properties**

<a href="http://i.imgur.com/3bG7swW.png">
  <img src="http://imgur.com/3bG7swWl.png"/>
</a>

**CloudFlare Settings**

<a href="http://i.imgur.com/fB6p2BG.png">
  <img src="http://imgur.com/fB6p2BGl.png" />
</a>

#### FFMPEG

As discussed in the Workflow section, `ogg` and `m4a` versions are required to allow the site to provide [in browser playback](https://github.com/RubberDucking/rubberduckcast.com/blob/master/_layouts/episode.html#L10-L14) via the `audio` tag. We use [FFMPEG](https://www.ffmpeg.org/) to accomplish the conversion from `m4a` to `ogg` because it's open source and has cross platform support. Make sure it's [installed](https://www.ffmpeg.org/download.html) on your machine.

#### The Codebase

Clone or download our [open source podcast site code](https://github.com/RubberDucking/rubberduckcast.com) and start modifying it to suit your needs! The [README](https://github.com/RubberDucking/rubberduckcast.com/blob/master/README.md) has more detailed descriptions of all the Grunt tasks.

We're trying to figure out the best way to open source the podcast site repo and make it more generally applicable. If you have any ideas on the best way to accomplish that let me know in the comments!
