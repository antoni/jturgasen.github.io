---
layout: post
title: "Syntax Test 3"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Paragraphs:  
Sentence one, line one, no double spaces after it.
Sentence two, line two, no double spaces after it.
Sentence three, line three, no double spaces after it.
This is line four and it should all get mooshed together.

Or, you can type it normally on a single line.  Like this.  See it?  Yep that works too.

Here I use a bunch of br br br to force line breaks
<br><br><br><br><br>

Headers:  

# Level 1, headers must be proceeded by a blank line
stuff  
more stuff  

## Level 2 header using two hashes
stuff  
more stuff  

### Level 3 header, up to level 6
lookin good  
still lookin good  


List:  

* Must be proceeded by a blank line
* item 2  
* item 3  


Ordered List:  

1. Must be proceeded by a blank line
2. Two  
3. Three  


List with paragraphs:  

* paragraph one  
paragraph two  
paragraph three  
* paragraph one  
paragraph two  
paragraph three  



Blockquotes:  

> must be proceeded by a blank line  
> that  
> thing  
> that  
> happened  


Pre-formatted Code Block:  
The world's best shell script:  

	#!/bin/bash  
	# must have blank lines above and below this code block
    echo foo  
    exit 0  

Wasn't that great?  


Inline Code:  
There are backticks around binbash `#!/bin/bash` words words  
Surround a backtick with pairs of backticks to display it or escape it: `` ` ``  


Links:  
[good old example.com](http://www.example.com/)  
[good old example.com with title attribute](http://www.example.com/ "blahblahblah")  
<http://www.example.com> is an "automatic link"  
<test@example.com> is an automatic e-mail link  
[e-mail me](mailto:test@example.com) is a normal e-mail link
  


Horizontal Rule:  
above, using 3 or more hyphens  
--------------------------------------------  
below  
As an alternative you can use 3 or more underscores or asterisks, more == longer  


Text formatting: 
*Single asterisk or underscore for italics*  
**Double asterisks or underscores for bold**  
->centered<-


Backslash Escapes:  
\\  
\`  
\*  
\_  
\{  
\#  
And so on  


Inline Images:  
![This is alt text](/assets/ls41bDB.jpg "optional title here")  
local, working image

![IMAGE NOT FOUND](/assets/beepboop443gfdgdf.jpg)  
local, missing image

![Image Not Found!](http://media.tumblr.com/7f6c38418dadcc2851d17c859bbbdab5/tumblr_inline_nbk7zkS5FX1raprkq.gif)  
external GIF


Tables:  

| First Header  | Second Header |
| ------------- | ------------- |
| Row1 Cell1    | Row1 Cell2    |
| Row2 Cell1    | Row2 Cell2    |