---
layout: post
title: "RequireJS Projects and Asynchronously Loading the Google Maps API"
description: "A pattern that is useful for handling dependencies which asynchronously load their own dependencies. Like google maps."
category: Web Development
tags: [web development, tips/tricks]
---
{% include JB/setup %}

##The Problem

Recently I was talking with my good friend [Adam Jarret](http://adamjarret.com/) about some bugs he was running into with Google Maps while attempting to load the JavaScript API in a project that used [RequireJS](http://requirejs.org/).

The script responsible for loading the Google Maps API does so by making calls to several other scripts, all of which must be loaded before the Google Maps API can be used. If you're using Google Maps in your project you're probably familiar with this URL:


`https://maps.googleapis.com/maps/api/js?key=API_KEY&sensor=SET_TO_TRUE_OR_FALSE`

The prevailing dicussions on the interwebs supported using the [requirejs async plugin](https://github.com/millermedeiros/requirejs-plugins/blob/master/src/async.js) to satisfy the dependency. Loading Google Maps with this plugin has two major problems:
 
 - It's not really compatible with the [optimizer](http://requirejs.org/docs/optimization.html).
 - There is no provided way to see if a given "async dependency" has actually **finished** loading. Especially if the dependency being loaded might make its own additional async loading calls.
 
 Because the API link above loads additional files by design, we needed to ensure that GoogleMaps was totally finished loading before trying to use it in the app, even when using optimized code. We also wanted to make sure that including the dependency across multiple files didn't cause additional unnecessary script load requests to be fired.
 

<script src="https://gist.github.com/MattSurabian/7868115.js"></script>

 
 