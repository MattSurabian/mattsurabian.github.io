+++
date = "2013-12-09T17:00:00+09:00"
title = "RequireJS Projects and Asynchronously Loading the Google Maps API"
description = "A pattern that is useful for handling dependencies which asynchronously load their own dependencies. Like google maps."
thumbnail = "images/gmaps.png"
+++

##The Problem

Recently I was talking with my good friend [Adam Jarret](http://adamjarret.com/) about some bugs he was running into with Google Maps while attempting to load the JavaScript API in a project that used [RequireJS](http://requirejs.org/).

The script responsible for loading the Google Maps API does so by making calls to several other scripts, all of which must be loaded before the Google Maps API can be used. If you're using Google Maps in your project you're probably familiar with this URL:


`https://maps.googleapis.com/maps/api/js?key=API_KEY&sensor=SET_TO_TRUE_OR_FALSE`

The prevailing dicussions on the interwebs supported using the [requirejs async plugin](https://github.com/millermedeiros/requirejs-plugins/blob/master/src/async.js) to satisfy the dependency. Loading Google Maps with this plugin has two major problems:
 
 - It's not really compatible with the [optimizer](http://requirejs.org/docs/optimization.html).
 - There is no provided way to see if a given "async dependency" has actually **finished** loading. Especially if the dependency being loaded might make its own additional async loading calls.
 
 Because the API link above loads additional files by design, we needed to ensure that Google Maps was totally finished loading before trying to use it in the app, especially when using optimized code. We also wanted to make sure that including the dependency across multiple files didn't cause additional unnecessary script load requests to be fired.
 
##The Solution

You had to know we were going to use [promise objects](http://api.jquery.com/promise/), right? 

The Google Maps API URL noted above accepts a callback querystring param. This callback param is not used for JSONP data delivery as you might expect, but rather it's called when the Google Maps API is fully loaded! Armed with this knowledge it was a reasonably straightforward excercise setting up a RequireJS module that lazy loaded and delivered the Google Maps API while still playing nice with the optimizer and allowing for some visibility into the loading process.

The gist starts with some boiler plate code to demonstrate how to use it. The ideal architectural pattern to use here is what I refer to as a "bootstrapping pattern". Essentially declaring the Google Maps loader dependency as high up in your call stack as possible, and not attempting to start your app code until the deferred has resolved. This saves you from having to declare the Google Maps loader as a dependecy on every view or file in your app that might need it. If you don't start the app code until the returned promise is resolved you can confidently use the `google.maps` object in your code and know it will be available.

However, this module is smart enough to only attempt loading the script (and all its friends) a single time. Meaning that if you'd rather include the Google Maps loader module in every script that's trying to access the Google Maps API, as the boiler plate suggests, it'll be easy and work as it should.

Note that the gist sets the sensor param as true, and does not define an API key on the URL. Make sure to adjust the loading url as necessary for your project.

<script src="https://gist.github.com/MattSurabian/7868115.js"></script>

 
 