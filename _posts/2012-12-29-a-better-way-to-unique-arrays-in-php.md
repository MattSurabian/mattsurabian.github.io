---
layout: post
title: "A Better Way To Unique Arrays In PHP"
description: "Are you uniquing PHP arrays using array_unique?  Consider doubling up with array_flip instead.  Array_unique is pretty slow."
category: PHP
tags: [Web Development, PHP, Performance, Tips]
---
{% include JB/setup %}

Are you uniquing PHP arrays using [array_unique](http://php.net/manual/en/function.array-unique.php)?  Consider doubling up with array_flip instead.  Array_unique is [pretty slow](http://stackoverflow.com/questions/8321620/array-unique-vs-array-flip/8321709#8321709).

<script src="https://gist.github.com/MattSurabian/5521979.js"></script>