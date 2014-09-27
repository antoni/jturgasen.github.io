---
layout: post
title: "Syntax Test 2"
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


Headers:  
# Level 1 header using one hash  
stuff  

## Level 2 header using two hashes
stuff  

### Level 3 header, closed with three hashes at the end
lookin good  


List:  
+ item 1  
+ item 2  
+ item 3  


Ordered List:  
1. One  
2. Two  
3. Three  


List with paragraphs:  
* thing #1  
paragraph two  
line three  
* thing #2  
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
The world's best shell script:  
    #!/bin/bash  
    echo foo  
    exit 0  
Wasn't that great?  


Inline Code:  
Use backticks `#!/bin/bash` words words  
``I need to display a backtick `whoami` so I used double backticks around the entire sentence``  
Using backticks to display a backtick: `` ` ``  


Links:  
[good old example.com](http://www.example.com/)  
[good old example.com with title attribute](http://www.example.com/ "blahblahblah")  
<http://www.example.com> is an "automatic link"  
<test@example.com> is an automatic e-mail link  


Reference Link:  
[good old example.com][1] is the best for testing.  [google.com][abc-123] is good too.  
[1]: http://www.example.com "Optional title text"  
[abc-123]: https://www.google.com "A link to Google.com"  


Horizontal Rule:  
above, using 3 or more hyphens  
--------------------------------------------  
below  
As an alternative you can use 3 or more underscores or asterisks, more = longer  


Bold, Italics, Strikethrough  
*Single asterisk or underscore for italics*  
**Double asterisks or underscores for bold**  
~~Tilde tilde~~ to strikethrough


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

![IMAGE NOT FOUND](/assets/beepboop443gfdgdf.jpg)  
local, missing image

![Image Not Found!](http://media.tumblr.com/7f6c38418dadcc2851d17c859bbbdab5/tumblr_inline_nbk7zkS5FX1raprkq.gif)  
external GIF


Tables:  
| First Header  | Second Header |
| ------------- | ------------- |
| Row1 Cell1    | Row1 Cell2    |
| Row2 Cell1    | Row2 Cell2    |