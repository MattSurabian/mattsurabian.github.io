+++
date = "2012-03-05T17:00:00+09:00"
title = "Validating Multiple Input Options in PHP"
description = "While pair programming last week a colleague of mine showed me a great trick for validating user input with multiple options. This trick works best in PHP because of its fantastic array support. But with the right utility classes or frameworks you could really use it with any language..."
thumbnail = "images/options.jpg"
+++

<script async custom-element="amp-gist" src="https://cdn.ampproject.org/v0/amp-gist-0.1.js"></script>

While pair programming last week a colleague of mine showed me a great trick for validating user input with multiple options. This trick works best in PHP because of its fantastic array support. But with the right utility classes or frameworks you could really use it with any language. It just might not save you as much time as it does in PHP.

In this case we were writing some setters for a class. The input had two valid values “allow” and “deny”. The normal way I would do this is with an OR inside an if statement. Maybe you’d want to convert it to lowercase before the if block.

<pre><code>if($input == "allow" || $input == "deny")</code></pre>

So this works fine, and in this situation there will probably not be any other inputs that would need to be checked. Fast forward to another setter. This one can take 7 values:

<pre><code>if($input == "always" || $input == "hourly" || $input == "daily" || $input == "weekly" || $input == "monthly" || $input == "yearly" || $input == "never")</code></pre>

Ok, now I’m wishing there was an easier way to do this. Enter my colleague’s amazing tip:

<amp-gist
    data-gistid="5522097"
    layout="fixed-height"
    height="225">
</amp-gist>

To be honest I was embarrassed it hadn’t occurred to me before now. Both strtolower and in_array are VERY cheap operations in PHP, so using this to validate inputs with multiple acceptable values is a no brainer. As illustrated above I usually check the inverse of the if statement and throw an error or exception.