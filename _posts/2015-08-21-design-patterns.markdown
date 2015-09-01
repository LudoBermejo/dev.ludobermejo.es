---
layout: post
title:  "Creational pattern"
date:   2015-09-01 08:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/instacode.png"
---

This design pattern conforms the base of many other designs patterns. Usually it is the first pattern that people study because is one of the simplest.

So, `what is this pattern about`? Well, this plays with the `creation`, mostly of objects. You know, in Javascript there is a few ways to create things, like:

    var thisIsMyObject = new Object();
    var thisIsAnotherObject = {};
    var thisIsMyThirObject = Object.create(null); // I only saw this creation on tutorial pages.  

As you can see, this is possibly the most easy pattern we can learn. In the above example we just create an empty object, `without any properties`.

Now, how can we add some props to a created object? Let's check some ways:
 
    var houseOfGameOfThrones = new Object(); // we create the object
    
    // First way
    houseOfGameOfThrones.name = "Baratheon"; // we assign a property called name
    console.log(houseOfGameOfThrones.name); // this is one way to access the property
    
    
    // Second way
    houseOfGameOfThrones["numberOfDeathsInBooks"] = "About five";
    console.log(houseOfGameOfThrones["numberOfDeathsInBooks"]); // this is another way to access the property
    
    // Third way. ATTENTION: Only for ECMASCRIPT 5 browsers/servers
    Object.defineProperty(houseOfGameOfThrones, "actualLeader", {
        configurable: true,
        writable: true,
        enumerable: true,
        value: "...it's complicated",
    });
    
    // Fourth way. Very similar to third, but for multiple properties:
    
    Object.defineProperties(houseOfGameOfThrones, {
            "numberOfHeirs": {
                value: "Who nows",
                writable: true
            },
            
             "previousLeader": {
                value: "Rober Barateon",
                writable: false
            }
        }
    );


Again, we can use different approaches to make properties. But remember that the `third and the fourth way` can only be used on [ECMASCRIPT 5][http://kangax.github.io/compat-table/es5/] compatible browsers and servers. Yeah, I know, they are a little verbose, but they are veeery impressive and you can use them to make your app more easy to follow when it has multiple objects. 