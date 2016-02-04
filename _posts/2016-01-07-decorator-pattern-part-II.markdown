---
layout: post
title:  "Decorator pattern (part II): Abstract Decorators"
date:   2016-02-04 19:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/decoratorpartone.jpg"
---

*Note: If you come here for the first time, you might want to <a href="/patterns/2016/01/07/decorator-pattern-part-I.html">read my first post</a>.*
 
Today we will learn about a variant of the Decorator pattern, more powerful and definitely more complex. I don't recommend this pattern for an usual app, but it can help for complex apps that needs to be coded by multiple developers.

I don't know if you are familiar with the concept of `interface`. It's a basic thing in the C++ world and one of the best things we have in OOP to make things with sense. With an interface you define how what are the methods that needs to have an object, but it doesn't implement any of this methods. So this is perfect to make, well, interfaces for classes. And that's can solve some problems we might find with the Decorators, cos you can define what kind of objects can be added to this decorator. So imagine you want to add some kind of car feature (like openDoor or brakesOn) to a animal. It makes no sense, right? Well, if we use this pseudo-interface we can control if one object will support the Decorator. And in Javascript we can use this feature with the `Abstract decorator pattern`.

Now, for use this, we need first to create an Interface class. I will copy the code from stackoverflow because it is a very simple code. I'll add comments where I think the code is unclear.


    // First we create the interface: a name and a list of methods that the object must have to be approved
    var Interface = function(name, methods) {
        if (arguments.length != 2) {
            throw new Error("Interface constructor called with " + arguments.length + "arguments, but expected exactly 2.");
        }
    
        this.name = name;
        this.methods = [];
    
        for (var i = 0, len = methods.length; i < len; i++) {
            if (typeof methods[i] !== 'string') {
                throw new Error("Interface constructor expects method names to be " + "passed in as a string.");
            }
    
            this.methods.push(methods[i]);
        }
    };
    
    // Then we create a function that checks an object with the methods that must have. If the object doesn't have the methods, then it throws an error.
    Interface.ensureImplements = function(object) {
        if (arguments.length < 2) {
            throw new Error("Function Interface.ensureImplements called with " + arguments.length + "arguments, but expected at least 2.");
        }
    
            
        for (var i = 1, len = arguments.length; i < len; i++) {
            var interface = arguments[i];
    
            if (interface.constructor !== Interface) {
                throw new Error("Function Interface.ensureImplements expects arguments" + "two and above to be instances of Interface.");
            }
    
            for (var j = 0, methodsLen = interface.methods.length; j < methodsLen; j++) {
                var method = interface.methods[j];
    
                if (!object[method] || typeof object[method] !== 'function') {
                    throw new Error("Function Interface.ensureImplements: object " + "does not implement the " + interface.name + " interface. Method " + method + " was not found.");
                }
            }
        }
    };


Ok, now let's check the example


    // First we create the interface: a name and a list of methods that the object must have to be approved
    var Interface = function(name, methods) {
        if (arguments.length != 2) {
            throw new Error("Interface constructor called with " + arguments.length + "arguments, but expected exactly 2.");
        }

        this.name = name;
        this.methods = [];

        for (var i = 0, len = methods.length; i < len; i++) {
            if (typeof methods[i] !== 'string') {
                throw new Error("Interface constructor expects method names to be " + "passed in as a string.");
            }

            this.methods.push(methods[i]);
        }
    };

    // Then we create a function that checks an object with the methods that must have. If the object doesn't have the methods, then it throws an error.
    Interface.ensureImplements = function(object) {
        if (arguments.length < 2) {
            throw new Error("Function Interface.ensureImplements called with " + arguments.length + "arguments, but expected at least 2.");
        }


        for (var i = 1, len = arguments.length; i < len; i++) {
            var interface = arguments[i];

            if (interface.constructor !== Interface) {
                throw new Error("Function Interface.ensureImplements expects arguments" + "two and above to be instances of Interface.");
            }

            for (var j = 0, methodsLen = interface.methods.length; j < methodsLen; j++) {
                var method = interface.methods[j];

                if (!object[method] || typeof object[method] !== 'function') {
                    throw new Error("Function Interface.ensureImplements: object " + "does not implement the " + interface.name + " interface. Method " + method + " was not found.");
                }
            }
        }
    };


    // First we define an interface (name and methods). That's the minimal methods that any object needs to have.
    var SpaceShipInterface = new Interface('Spaceship', [ 'fly', 'flyInSpace' ]);


    // Next I'm creating an object with the methods prepared
    var Spaceship = function() {
        this.fly  = function() {
            return "can fly";
        },
        this.flyInSpace = function() {
            return "can fly in space";
        }
    }



    // Now i'm creating star wars space ship decorator. It only works if the passed object can fly and flyinspace
    var StarwarsSpaceShipDecorator = function(ship) {
        Interface.ensureImplements(ship, SpaceShipInterface);
        ship.goHyperspace =  function() {
            return "can go hyperspace"
        },
        ship.firePhasers =  function() {
            return  "can fire phasers"
        }
    }

    // Now lest create a spaceship
    var starwarsShip = new Spaceship();
    console.log(starwarsShip);

    // and let's add the decorator

    StarwarsSpaceShipDecorator(starwarsShip);

    // Everything's ok, the object can fly, fly in space, go into hyperspace and fire phasers
    console.log(starwarsShip);

    // Now I'm creating a airplane
    var AirPlane = function() {
        this.fly  = function() {
            return "can fly";
        }
    }

    var airplane = new AirPlane();

    // I'll try to add the decorator
    StarwarsSpaceShipDecorator(airplane);

    // But ey, it doesn't work! It give us:
    // Uncaught Error: Function Interface.ensureImplements: object does not implement the Spaceship interface. Method flyInSpace was not found.


Great! Now I think this is much clear now, right? You can combine this with prototypes and it will be even better :)

 
 