---
layout: post
title:  "Arrow functions"
date:   2016-04-06 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/letandconstandblock.jpg"
---

Oh dear! Arrow functions was one of the more controversial updates of the ECMA6. Or at least, one of the most popularized discussion. Are they [Syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar) or are they really useful? Spoiler: IMMO they are useful and have some things awesome, but you can convert your code in a hell if you don't have enough care.
 
But, again, what are the `Arrow functions`? Well, they are a new way to define and use functions, with two things in mind:

- Remove the verbosity of functions code
- Remove the closures problems inherent to classic functions.

Let's see an example of an old function

    var whoIsMiyazaki = function() {
        return "A genial director"; 
    }
    
    console.log(whoIsMiyazaki());
    
This will return, as expected
    
    A genial director
    
Now let's tranform this function into an arrow function
    
    var whoIsMiyazaki = () => "A genial director";
    console.log(whoIsMiyazaki());

This is the result    

    A genial director

You can start the screaming now. What the f*ck is that? I know, first time you see this new annotation is like "oh my god". But you only need to check this:

- First, we remove the function and use only the parenthesis: **()**
- Then, you remove the curly braces {}
- You establish the assignation with **=>**
- You can remove the return statement too

Ok, it's confusing. But believe me: it works, and it works nice. Let's see another example:

    var takeAPill = function(pill) {
        return pill=="Blue"?"Continue with your live, Neo":"Become the chosen one";
    }

    console.log(takeAPill("Blue"));
    console.log(takeAPill("Red"));
    
This code will return:
    
    Continue with your live, Neo
    Become the chosen one

Let's try to convert it to arrow function:

    var takeAPill = pill => pill=="Blue"?"Continue with your live, Neo":"Become the chosen one";
    console.log(takeAPill("Blue"));
    console.log(takeAPill("Red"));
        
And see the result:
    
    Continue with your live, Neo
    Become the chosen one

Ok, what the f*uck happened with the parenthesis of the previous case? Well, I cheated: if you have only a parameter you don't need the parenthesis. Less code in the same space.  
