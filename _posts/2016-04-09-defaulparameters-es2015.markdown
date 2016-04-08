---
layout: post
title:  "Default parameter in ES2015"
date:   2016-04-09 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/letandconstandblock.jpg"
---

Ok this may be kind of absurd for many developers, but javascript didn't have default parameters in functions. Our usual code usually includes something similar to this:

    var giveMeTheTrueNameOf = function(name) {
        name = name || "Superman";
        switch(name) {
            case "Superman": return "Clark Kent"; break;
            case "Bruce Wayne": return "Batman"; break;
        }
    }

    console.log(giveMeTheTrueNameOf());
    console.log(giveMeTheTrueNameOf("Bruce Wayne"));
        
that give us this result:
        
    Clark Kent
    Batman
        
Yes, this `name = name || "Superman"` is the key to use the default values. But now, check this new and shinny function:
         
    var giveMeTheTrueNameOf = function(name = "Superman") {
        switch(name) {
            case "Superman": return "Clark Kent"; break;
            case "Bruce Wayne": return "Batman"; break;
        }
    }

    console.log(giveMeTheTrueNameOf());
    console.log(giveMeTheTrueNameOf("Bruce Wayne"));
    
Oh dear! At least! But, can we do it with multiple params?

    var whoIsYourDaddy = function ( name = "Luke", surname = "Skywalker" ) {
        if(name == "Luke" && surname == "Skywalker") return "Darth baddy vader!";
        return "No idea";
    }
    
    console.log( whoIsYourDaddy() );
    
Give us this result    

    Darth baddy vader!
    
But, what Javascript do to get the default value? Null? Undefined? Let's see it with the previous code
    
    
    var whoIsYourDaddy = function ( name = "Luke", surname = "Skywalker" ) {
        if(name == "Luke" && surname == "Skywalker") return "Darth baddy vader!";
        return "No idea";
    }
    
    console.log( whoIsYourDaddy(null, "Skywalker") );
    console.log( whoIsYourDaddy('', "Skywalker") );
    console.log( whoIsYourDaddy("Luke", undefined) );
    
This code will return:
    
    No idea
    No idea
    Darth baddy vader!

So javascript uses "undefined" to identify when you must use the default value.
    
Now let's do a complex thing:

    var giveMeYourName = function(name = "Luke", surname = "Skywalker", completeName = name + surname) {
        return "My complete name is " + completeName;    
    }

    console.log(giveMeYourName());
    
Ok, this is totally strange! What do you think is the result? Well, it's:
        
    My complete name is Luke Skywalker
    
And what if we put a name?    

    console.log(giveMeYourName("Leia"));
        
Well, as expected:
        
    My complete name is Leia Skywalker
        
And what about the surname?        
        
    console.log(giveMeYourName(undefined, "Skywalker"));
        
Yeah, you nail it:
        
    My complete name is Luke Skywalker
        
And know the freaking one:
        
    console.log(giveMeYourName("Anakin", "Skywalker", "Darth vader"));

Mmm... what's this? But the completeName is calculated in the header so... what's the result?

    My complete name is Darth vader
    
Oooooh.... incredible, right? Parameters can be dynamic and they will respect the new parameters or they will use default values.
 
You can even do strange thinks like:
 
    var giveMeMyShip = name => name=="Han solo"?"Millenium falcon":"No idea";
    
    var showNameOfShipsByNameOfOwner = function(nameOfOwner = "Han solo", ship = giveMeMyShip(nameOfOwner)) {
        console.log(ship)
    }

   
    
Ok, what happened here? First, I have declared an arrow function that returns "Millenium falcon" if the name of the owner is "Han solo". Then, I created a function with two parameters, both with default values: one is the name of the owner, the other one will calculate the result based on the name of the owner. Now let's see what happens:

    showNameOfShipsByNameOfOwner("Neo");
    
will return:
    
    No idea
           
And 
           
    showNameOfShipsByNameOfOwner("Han solo");
           
will return
           
    Millenium falcon
           
And the last example:
           
    showNameOfShipsByNameOfOwner();
          
will return, too:
          
    Millenium falcon

Cool right?
          
But, what if we do it in the other order? What will happend if we use this code?
          
    var giveMeMyShip = name => name=="Han solo"?"Millenium falcon":"No idea";
    
    var showNameOfShipsByNameOfOwner = function(ship = giveMeMyShip(nameOfOwner), nameOfOwner = "Han solo") {
      console.log(ship)
    }

    showNameOfShipsByNameOfOwner();
    
Well, then we get an error:
    
    Uncaught ReferenceError: nameOfOwner is not defined
    
Because the nameOfOwner is not defined, yet! It will defined after the call

Ok, enough of this! I hope you find this as awesome as I did when I discovered it :)


    