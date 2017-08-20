+++
date = "2012-12-29T17:00:00+09:00"
title = "A Better Way To Unique Arrays In PHP"
description = "Are you uniquing PHP arrays using array_unique?  Consider doubling up with array_flip instead.  Array_unique is pretty slow."
thumbnail = "images/tire-flip.jpg"
ampscripts = """
<script async custom-element="amp-gist" src="https://cdn.ampproject.org/v0/amp-gist-0.1.js"></script>
"""
+++

Are you uniquing PHP arrays using [array_unique](http://php.net/manual/en/function.array-unique.php)?  Consider doubling up with array_flip instead.  Array_unique is [pretty slow](http://stackoverflow.com/questions/8321620/array-unique-vs-array-flip/8321709#8321709).

<amp-gist
    data-gistid="5521979"
    layout="fixed-height"
    height="225">
</amp-gist>
