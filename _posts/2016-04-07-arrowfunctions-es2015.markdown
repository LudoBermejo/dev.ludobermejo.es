---
layout: post
title:  "Arrow functions"
date:   2016-04-07 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/arrowfunctions.jpg"
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
  
But you know it could not be so easy. So, what will you do when you have multiple parameters? Let's see it in the old style

    var makeMeAJaeger = function( name, pilot1, pilot2) {
        return "I asm " + name + ", piloted by " + pilot1 + " & " + pilot2;
    }

    console.log(makeMeAJaeger( "Gipsy Danger", "Mako Mori", "Yancy Becket"));
    
It will show
    
    I asm Gipsy Danger, piloted by Mako Mori & Yancy Becket
    
Now, how can we do it in an arrow function? Let's see it
     
    var makeMeAJaeger = ( name, pilot1, pilot2) => "I asm " + name + ", piloted by " + pilot1 + " & " + pilot2;
    console.log(makeMeAJaeger( "Gipsy Danger", "Mako Mori", "Yancy Becket"));     

It will show 

    I asm Gipsy Danger, piloted by Mako Mori & Yancy Becket

Ok, so when you need to use more than one parameter, you need to add the parenthesis. I know it's a little tricky but, hey, you can use the parenthesis with only just param. Just saying!

Now move on! What if I want to use more that one sentece? Can we still use arrow functions? Oh, of course you do. Just like this:

    var makeMeAJaeger = ( name, pilot1, pilot2) => {
        return "I asm " + name + ", piloted by " + pilot1 + " & " + pilot2;
    }
    console.log(makeMeAJaeger( "Gipsy Danger", "Mako Mori", "Yancy Becket"));
    
As you can see, you need to use again the `return` keyword and the curly braces. 

I know what you are thinking... "ok, Ludo at the end of the day, this is no more that syntactic sugar, right?". Well... no. Let's see one of the biggest differences between arrow functions and normal functions: the `scope`:
  
This is a normal, normal code:

    <input type="button" id="BluePill" value="Take the blue pill">
    <input type="button" id="RedPill" value="Take the red pill">
    
    <script>
        'use strict';
    
        var neoDecision = {
            decision: "",
            takeAPill: function(pill) {
                return pill=="Blue"?"Continue with your live, Neo":"Become the chosen one";
            },
            init: function() {
                document.getElementById("BluePill").addEventListener('click', function() {
                    console.log(this.takeAPill("Blue"));
                })
    
                document.getElementById("RedPill").addEventListener('click', function() {
                    console.log(this.takeAPill("Red"));
                })
            }
        }
    
        neoDecision.init();
    
    </script>
    
When I click to any button... what will I get? 
    
    Uncaught TypeError: this.takeAPill is not a function
    
Ah! The `scope` and the `closures`! The eternal problem of javascript. Let's see a "valid" code    

    <input type="button" id="BluePill" value="Take the blue pill">
    <input type="button" id="RedPill" value="Take the red pill">
    
    <script>
        'use strict';
    
        var neoDecision = {
            decision: "",
            takeAPill: function(pill) {
                return pill=="Blue"?"Continue with your live, Neo":"Become the chosen one";
            },
            init: function() {
                var self = this;
                document.getElementById("BluePill").addEventListener('click', function() {
                    console.log(self.takeAPill("Blue"));
                })
    
                document.getElementById("RedPill").addEventListener('click', function() {
                    console.log(self.takeAPill("Red"));
                })
            }
        }
    
        neoDecision.init();
    
    </script>
    
Ok, do you see what I did? I put the self variable pointing to the object, and then I use it inside the click event. Easy enough but it breaks the head of a lot of developers. 
    
Now, what can we do with arrow functions?

    <input type="button" id="BluePill" value="Take the blue pill">
    <input type="button" id="RedPill" value="Take the red pill">
    
    <script>
        'use strict';
    
        var neoDecision = {
            decision: "",
            takeAPill: pill => pill=="Blue"?"Continue with your live, Neo":"Become the chosen one",
            init: function() {
                document.getElementById("BluePill").addEventListener('click', () => {
                    console.log(this.takeAPill("Blue"));
                })
    
                document.getElementById("RedPill").addEventListener('click',  () => {
                    console.log(this.takeAPill("Blue"));
                })
            }
        }
    
        neoDecision.init();
    
    </script>
   
It will return the correct values when you click the buttons. 
   
But, why is that? That's because the arrow function `doesnt have a custom closure`. The "inherited" the parent scope. So you can forget the `var self = this`! Less code, less errors, less stress.
 
And... I think that's all for now about arrow functions. But remember... in the next posts I will use this feature continuously. So you will need to understand it properly :)
    