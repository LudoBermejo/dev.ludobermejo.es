---
layout: post
title:  "Rest & Spread"
date:   2016-04-12 12:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/default_parameters.jpg"
---

More cool things for ES2015! Today we will learn about the `Rest` and `Spread` operators in the new Javascript. Let's start!
 
# REST

With the rest syntax we can represent any number of parameters we use as an array. 

Let's start with an example:

    var gremlinsFamily = function(father, ...children) {
        console.log("The father of all: " + father)
        console.log("The children: ");
        for(var i in children) {
            console.log("-" + children[i]);
        }
    }
    
    gremlinsFamily("Gizmo", "Stripe", "Electricity Gremlin", "Shredder Gremlin", 
        "Vegetable Gremlin", "The Gremlin Carolers","The Phantom of the Opera Gremlin", 
            "Flashdance Gremlin", "Mohawk/Spider-Gremlin","Mugger Gremlin", 
            "Lady Gremlina", "Brain Gremlin", "Jazzy Gremlin" )

This will return

    The father of all: Gizmo
    The children: 
    -Stripe
    -Electricity Gremlin
    -Shredder Gremlin
    -Vegetable Gremlin
    -The Gremlin Carolers
    -The Phantom of the Opera Gremlin
    -Flashdance Gremlin
    -Mohawk/Spider-Gremlin
    -Mugger Gremlin
    -Lady Gremlina
    -Brain Gremlin
    -Jazzy Gremlin

How cool is that? I have created a function that will get all the parameters but the first in the "children" parameter. Just like that.

Now, let's add a second parameter:

    var gremlinsFamily = function(father, boss, ...children) {
        console.log("The father of all: " + father)
        console.log("The boss of bady guys: " + boss)
        console.log("The children: ");
        for(var i in children) {
            console.log("-" + children[i]);
        }
    }
    
    gremlinsFamily("Gizmo", "Stripe", "Electricity Gremlin", "Shredder Gremlin", 
        "Vegetable Gremlin", "The Gremlin Carolers","The Phantom of the Opera Gremlin", 
            "Flashdance Gremlin", "Mohawk/Spider-Gremlin","Mugger Gremlin", 
            "Lady Gremlina", "Brain Gremlin", "Jazzy Gremlin" )
            
It will return:
            
    The father of all: Gizmo
    The boss of bad guys: Stripe
    The children: 
    -Electricity Gremlin
    -Shredder Gremlin
    -Vegetable Gremlin
    -The Gremlin Carolers
    -The Phantom of the Opera Gremlin
    -Flashdance Gremlin
    -Mohawk/Spider-Gremlin
    -Mugger Gremlin
    -Lady Gremlina
    -Brain Gremlin
    -Jazzy Gremlin
            
Yes, I only have to create the second variable; it will auto fill it with the second parameter
             
But, what is this "children" var? Let's see it:
             
    var gremlinsFamily = function(father, boss, ...children) {
       console.log(children);
    }
    
    gremlinsFamily("Gizmo", "Stripe", "Electricity Gremlin", "Shredder Gremlin", 
        "Vegetable Gremlin", "The Gremlin Carolers","The Phantom of the Opera Gremlin", 
            "Flashdance Gremlin", "Mohawk/Spider-Gremlin","Mugger Gremlin", 
            "Lady Gremlina", "Brain Gremlin", "Jazzy Gremlin" )
             
It will return
             
    ["Electricity Gremlin", "Shredder Gremlin", "Vegetable Gremlin", 
    "The Gremlin Carolers", "The Phantom of the Opera Gremlin", 
    "Flashdance Gremlin", "Mohawk/Spider-Gremlin", "Mugger Gremlin", 
    "Lady Gremlina", "Brain Gremlin", "Jazzy Gremlin"]             
    
So this children is an array. And we can play with it as we want.
    
# SPREAD    
    
The spread operator allow us to use expressions that can be expanded when we expect multiple arguments. Let's explain it with an old example:

    function spellThis(first, second, third) {
        console.log("first is "+ first);
        console.log("Second is "+ second);
        console.log("Third is "+ third);
    }
    
    spellThis("N","E","O");
        
This will return:
        
    first is N
    Second is E
    Third is O
    
We can remove this sh*tty code for something beautiful like this:
         
    function spellThis(letters) {
    
        for(var i in letters) { console.log(letters[i]) }
    }
    
    spellThis("NEO IS THE CHOSEN ONE");
    
This will return:
    
    N
    E
    O
     
    I
    S
     
    T
    H
    E
     
    C
    H
    O
    S
    E
    N
     
    O
    N
    E
    
Can you see it? We can transform any object in something iterable. This has a lot of used, like this one:
    
    var oldSkyWalkerFamily = ["Daddy Vader", "Luke", "Leia"];
    var newSkyWalkerFamily = ["Kylo Ren", "Maybe Finn?"];
     
    oldSkyWalkerFamily.push(...newSkyWalkerFamily);
    
    console.log(oldSkyWalkerFamily);
    
Will return:
    
    ["Daddy Vader", "Luke", "Leia", "Kylo Ren", "Maybe Finn?"]
    
Cool...
    
Now a tricky one. What do you thing about this code?

    var spell = function (...what) {
        console.log(what)
    }
    
    spell(..."spell","and really",..."spell");

What do you think about this? Could you tell me what we will get?

    ["s", "p", "e", "l", "l", "and really", "s", "p", "e", "l", "l"]
    
It makes sense right? No? Well, you get three parameters: one array (because the spread of the word), then a string (and really) and finally another array spreaded.
     
# Conclusions
     
You may think all of this can be achieved in other ways and you might be right. But this could reduce the amount of lines you write, and it will surely reduce the complexity of this lines. So... welcome both, Rest and spread!     