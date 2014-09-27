---
layout: post
title: "Syntax Test"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Headers:
First level header using equals signs
====================
stuff

Second level header using dashes
-------------------
stuff

# Atx-style level 1 header using one hash
stuff

## Atx-style level 2 header using two hashes
stuff

### Atx-style level 3 header, closed with three hashes at the end ###
lookin good


List:
* item 1
* item 2
* item 3


Ordered List:
1. One
2. Two
3. Three


List with paragraphs:
* line one
line two
line three
* line one
line two
line three


List with blockquotes:
* he said
> blah blah
* she said
> blah2 blah2


Blockquotes:
> that
> thing
> that
> happened


Pre-formatted Code Block:
Here's the world's best shell script:
	#!/bin/bash
	echo foobar
	exit 0
Wasn't that great?


Inline Code:
Use backticks `#!/bin/bash` words words
``I need to display a backtick `whoami` so I used double backticks around the entire sentence``
Using backticks to display a backtick: `` ` ``


Links:
[good old example.com](http://www.example.com/)
[good old example.com with title attribute](http://www.example.com/ "Example.com word word")
<http://www.example.com> is an "automatic link"
<test@example.com> is an automatic e-mail link


Reference Link:
[good old example.com][1] is the best for testing.  [google.com][abc-123] is good too.
[1]: http://www.example.com "Optional title text"
[abc-123]: https://www.google.com "A link to Google.com"


Horizontal Rule:
above, using 3 or more hyphens
---
below
As an alternative you can use 3 or more underscores or asterisks


Emphasis:
*I need an adult!*
**I really need an adult!**
You can use underscores instead of asterisks


Backslash Escapes:
\\
\*
\`
\_
\{
\#
And so on


Inline Images:
![This is alt text](/assets/ls41bDB.jpg "optional title here")
local, working image

![Image Not Found!](/assets/beepboop443gfdgdf.jpg)
local, missing image

![Image Not Found!](http://media.tumblr.com/7f6c38418dadcc2851d17c859bbbdab5/tumblr_inline_nbk7zkS5FX1raprkq.gif)
external GIF