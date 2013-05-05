---
layout: post
title: "Working With Poorly Documented APIs"
description: "APIs are awesome!  Being able to tap into a web service and work with data can make a lot of tasks easier and cleaner.   APIs also allow us to link into advanced functionality and provide a richer experience for end users.  Unfortunately, a lot of APIs are poorly documented.  Here's how to deal with it..."
category: Web Development 
tags: [Web Development, Tips]
---
{% include JB/setup %}

APIs are awesome!  Being able to tap into a web service and work with data can make a lot of tasks easier and cleaner.   APIs also allow us to link into advanced functionality and provide a richer experience for end users.  Unfortunately, a lot of APIs are poorly documented, even ones for well known online services (you know who you are).  This can make working with them challenging, and debugging their errors even more of a nightmare.  This is where our old friend _try{…}catch{…}_ can come to our aid.

If you’re unfortunate enough to be working with a poorly documented API in a language that doesn’t support exceptions my heart goes out to you.  Fortunately many languages do support exceptions and the sooner you embrace working with them the better off you’ll be.

Throw A Custom Exception
------------------------
A lot of times documentation fails us by not giving us enough information about what we can send to the API and what can expect to receive back from it, forcing us to make assumptions in our code.  Through testing and reverse engineering it’s not uncommon for most programmers to work through these challenges and come out on the other side with functional code that plays nicely with the mystery API.  Unfortunately they’re also left with a ticking time bomb in their code.  Because this mystery API is undocumented or poorly documented any change to it could cause some of the code’s assumptions to no longer be correct.  Without documentation to fall back on it can be quite a task to track down the source of the error.

By acknowledging when these assumptions are being made, and wrapping some error catching code around them, you can save yourself lots of headaches in the future.  Implementing basic error handling adds almost no extra time to a project and makes tracking down problems a breeze, especially for other people that have to use your code or are less familiar with the API in question.

Let’s say you are planning on doing some parsing of the data the API sends back.  For arguments sake lets say the API spits back a comma delimited list and you’re going to break up that list into individual items and read them into a database or some other such thing.  Rather than just take the API’s undocumented word for it, why not toss a regex somewhere in there to confirm that your API has in fact sent you back a comma delimited list and not a cryptic error message, or a pipe delimited list.  If the API’s response isn’t as expected throw an exception that says exactly that.

Likewise listen for any error you know the API is prone to and throw a custom exception for each one.  That way you can gracefully handle them via a series of clean catch{…} blocks instead of a bunch of ridiculous test cases.

Real World Application
----------------------
Recently I was working with an API that accepted a resource ID and spit back content about this resource.  None of its possible errors were documented.  Through testing we discovered the two most often received errors were invalid parameter and resource moved.  In this instance invalid parameter meant that the database had gotten a hold of an incorrect resource ID somehow.  We already had methods to search for an ID if an element didn’t have one, so we just caught the error, wiped the ID, and let the program handle the rest.  Dealing with Resource Moved errors is where it gets interesting.  The API is kind enough to send back the new ID, but instead of sending it back in its own field it sends it back as part of the error message.  Something like: “This resource has been moved to <FULL URL>/<PATH>/<NEW RESOURCE ID>”.  My initial though was great, no problem, throw an exception, then just parse the string, snag the new resource ID and move on.

This is where acknowledging assumptions makes all the difference.  The project’s lead developer said, “You’re making an assumption here about the response from the API, what happens if that assumption isn’t true?  What should happen?”.  He was, of course, completely correct.  If the people responsible for the API came to their senses and just started returning the correct ID with the error message in a different format my string parsing code would be nonsense.  The answer in my case was immediately obvious, use a regex to ensure the response is of the expected format, if it is, parse the string, if it’s not throw an Invalid Parameter Exception so the program can cleanly recover.

Make sure when you’re implementing custom exceptions you put some type of output, preferably to a log, that indicates an error occurred, even if you have recovery code in place.  I’m partial to the phrase “Gracefully recovering from ____ error”, because it indicates that an error occurred but it’s nothing to worry about. Also pay attention to the order of your catch statements.  In my case I had to make sure my catch statement for Resource Moved came before my catch for Invalid Parameter.  Creating custom exceptions is really easy in most languages as it’s just extending the exception class, you don’t even have to add custom methods.