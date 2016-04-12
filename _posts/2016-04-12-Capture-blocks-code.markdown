---
layout: post
title:  "Capture blocks code: the project"
date:   2016-04-12 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/default_parameters.jpg"
---

Just a quick update to show you my new tool. You may know the difficulties that every developer on linkedin: you can't put a valid block of code because Linkedin doesn't have styles for them. So you can only do two things:
 
- Put the code in cursive or similar style
- Put an image instead the code

If you have seen my linkedin posts ([this can be an example](https://www.linkedin.com/pulse/strategy-pattern-ludo-bermejo-fernandez?trk=hp-feed-article-title-publish)) you can see I'm doing the second option. But this option forces me to capture every chunk of code manually. Yes, you know: I need to capture the full webpage and then I capture the chuck and save it to a file. 

At first it was all right but in the last posts I have more a more chunks and it is a hell to get it all properly. So I thought, there might be a simpler way to do this. And, after one hour of code, here it is: the Capture Blocks of Code or CBC (shitty name, yeah!)

Check the [repository](https://github.com/LudoBermejo/CaptureBlocksOfCode) and you will understand what I did:

a) First I capture the image with another tool

b) Then I drag and drop the image into my screen

c) I transform the image in black and white duotone.

d) Then seek for blobs of black and put their coordinates into an array

e) Finally I capture this blobs in the main page and zip them onto a file

Now you may be thinking: what the f**ck are he talking about? Let's see it on a video:
    
<iframe width="560" height="315" src="https://www.youtube.com/embed/eNi_5hhWnsA" frameborder="0" allowfullscreen></iframe>
 
# TODOs:
 
- The algorithm is poor; it looks for every pixel of a line; I would improve it by capturing a bigger block in order to get the pixels.
  
- A really UI would be wonderful
  
In any case, now I can add a new linkedin post without pain. And that's cool :)  
 
