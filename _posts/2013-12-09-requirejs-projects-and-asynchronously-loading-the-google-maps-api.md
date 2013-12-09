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
 
 