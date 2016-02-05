---
layout: post
title:  "Currying (Functional programming II)"
date:   2016-02-05 14:00:00
author: Ludo Bermejo
categories: functional 
tags:	functional 
cover:  "assets/functional_programming_part_1.jpg"
---

With this concept I still have problems. I can see how it works, I can see some uses, but I'm still learning why is so important in other languages as Haskell. Still I want to share with you my advancements because it's a very interesting stuff.

So... currying. What's that? `Currying` is a way of constructing functions that allows partial application of a function’s arguments. So you can pass all the arguments to a function and get the result, or only pass a subset of those arguments and get the function that is waiting the rest of the arguments.

Yeah, it's confusing, I know. Let's do an example so we can understand first the concept.
 
 
        function starTrekCharacter(name, planet, profession) {
            return "I'm " + name + " from the planet " + planet + " and I am a " + profession;
        }
    
        console.log(starTrekCharacter("Spock", "Vulcano", "science officer"))
        // returns I'm Spock from the planet Vulcano and I am a science officer

Easy enough. Now we are going to "disintrigrate" this function in multiple functions

    function starTrekCharacter(name) {
        return function(planet) {
            return function(profession) {
                return "I'm " + name + " from the planet " + planet + " and I am a " + profession;
            }
        }
    }

    console.log(starTrekCharacter("Spock")("Vulcano")("science officer"))
    // returns I'm Spock from the planet Vulcano and I am a science officer

Ok. Now we have three functions instead of one, and every function return another function except the last one; the last one returns the complete sentence. So we can do this

    var character = starTrekCharacter("Spock");
    var characterPlanet = character("Vulcano");
    console.log(characterPlanet("science officer"));
    
And you will get the same result. But that kind of code is ilegible so we can create a doItCurry function that transform a traditional function into a currying function.
    
    // This function creates a currying function of a usual function.
    var curry= function(f) {
        var paramsOfFunction = f.length;
        var args  = Array.prototype.slice.call(arguments, 1);

        adder = function() {
            var largs = args;

            if (arguments.length > 0) {
                // We must be careful to copy the `args` array with `concat` rather
                // than mutate it; otherwise, executing curried functions can have
                // strange non-local effects on other curried functions.
                largs = largs.concat(Array.prototype.slice.call(arguments, 0));
            }

            if (largs.length >= paramsOfFunction) {
                return f.apply(this, largs);
            } else {
                return curry.apply(this, [f].concat(largs));
            }
        };

        return (args.length >= paramsOfFunction) ? adder() : adder;
    };


    
And then use it on our first function:

    // This is the function we want to use
    function starTrekCharacter(name, planet, profession) {
        return "I'm " + name + " from the planet " + planet + " and I am a " + profession;
    }

    // We convert the original function to a currying one
    var starTrekCharacterCurrying = curry(starTrekCharacter);

    var character = starTrekCharacterCurrying("Spock");
    var characterPlanet = character("Vulcano");
    console.log(characterPlanet("science officer"));


So you migh be thinking... "ok Ludo, and now... what? What is this useful for?". So let's see an example with, for example, map.

Remember the game of thrones map example. It was like this:

        var gameOfThronesCharacters = [
             { name: "John Stark", house: "Unknown"},
             { name: "Catelyn Stark", house: "Tully"},
             { name: "Jorah Mormont", house: "Mormont"},
             { name: "Jaime Lannister", house: "Lannister"},
             { name: "Tyrion Lannister", house: "Lannister"},
             { name: "Sansa Stark", house: "Stark"},
             { name: "Robert Baratheon", house: "Baratheon"},
             { name: "Benjen Stark", house: "Stark"},
             { name: "Cersei Lannister", house: "Lannister"},
             { name: "Aria Stark", house: "Stark"}
        ]
        
        function getCharacterName(character) {
            return character.name;
        }
        
        function getCharacterHouse(character) {
            return character.name;
        }
         
        var names = gameOfThronesCharacters.map(getCharacterByName);
        console.log(JSON.stringify(names, null, 2)); 

        var houses = gameOfThronesCharacters.map(getCharacterByHouse);
        console.log(JSON.stringify(houses, null, 2)); 
names will return 

        [
          "John Stark",
          "Catelyn Stark",
          "Jorah Mormont",
          "Jaime Lannister",
          "Tyrion Lannister",
          "Sansa Stark",
          "Robert Baratheon",
          "Benjen Stark",
          "Cersei Lannister",
          "Aria Stark"
        ]
        
And houses will return
        
        [
          "Unknown",
          "Tully",
          "Mormont",
          "Lannister",
          "Lannister",
          "Stark",
          "Baratheon",
          "Stark",
          "Lannister",
          "Stark"
        ]
        
Ok now look at this:
        
       
    // This function creates a currying function of a usual function.
    var curry= function(f) {
        var paramsOfFunction = f.length;
        var args  = Array.prototype.slice.call(arguments, 1);

        adder = function() {
            var largs = args;

            if (arguments.length > 0) {
                // We must be careful to copy the `args` array with `concat` rather
                // than mutate it; otherwise, executing curried functions can have
                // strange non-local effects on other curried functions.
                largs = largs.concat(Array.prototype.slice.call(arguments, 0));
            }

            if (largs.length >= paramsOfFunction) {
                return f.apply(this, largs);
            } else {
                return curry.apply(this, [f].concat(largs));
            }
        };

        return (args.length >= paramsOfFunction) ? adder() : adder;
    };

    var gameOfThronesCharacters = [
        { name: "John Stark", house: "Unknown"},
        { name: "Catelyn Stark", house: "Tully"},
        { name: "Jorah Mormont", house: "Mormont"},
        { name: "Jaime Lannister", house: "Lannister"},
        { name: "Tyrion Lannister", house: "Lannister"},
        { name: "Sansa Stark", house: "Stark"},
        { name: "Robert Baratheon", house: "Baratheon"},
        { name: "Benjen Stark", house: "Stark"},
        { name: "Cersei Lannister", house: "Lannister"},
        { name: "Aria Stark", house: "Stark"}
    ]

    // We create a currying function that will return the id we want
    var getById = curry(function(property, object) {
        return object[property]
    });

    // And then we use it
    var names = gameOfThronesCharacters.map(getById('name'));
    var houses = gameOfThronesCharacters.map(getById('house'));


    console.log(JSON.stringify(names, null, 2));
    console.log(JSON.stringify(houses, null, 2));
        
This will return exactly the same.

    [
      "John Stark",
      "Catelyn Stark",
      "Jorah Mormont",
      "Jaime Lannister",
      "Tyrion Lannister",
      "Sansa Stark",
      "Robert Baratheon",
      "Benjen Stark",
      "Cersei Lannister",
      "Aria Stark"
    ]
    prueba.html:56 [
      "Unknown",
      "Tully",
      "Mormont",
      "Lannister",
      "Lannister",
      "Stark",
      "Baratheon",
      "Stark",
      "Lannister",
      "Stark"
    ]
        
Can you see it? With currying functions we can extend functions to do much more things. In the future I will explain more examples of this. But for now, I think this is enough :)