---
layout: post
title:  "Prototype pattern"
date:   2015-09-30 18:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/prototype-pattern.jpg"
---

Ok, I must confess that I never had a teacher who knew a thing about patterns. When I started college the book [Design Patterns: Elements of Reusable Object-Oriented Software](https://en.wikipedia.org/wiki/Design_Patterns) was a novelty and... well, my teachers were not good at all. But they know a lot about C++... I grow my teeth by developing classes with Turbo C++ and I learnt the pros and the cons of object oriented programming.
    
So when I arrived to Javascript, in the 2000 (yeah, I know), my first impulse was to make everything like C++. I made a framework, I tried to convince every partner that was the way to go... and I failed miserably, of course. I didn't have enough formation and the language was not so advanced like now.

And then, in the middle 2000s, all started to work. And I understood that Javascript is not C++, that has his owns merits and defects, and that we can get something even better that the usual inheritance. Like, for example, this prototype pattern. 

But, what is the prototype pattern? Well, that is the pattern that creates an object based on a template of an existing object by cloning it.

Simple enough, right?

The idea of this pattern is not new. So there are many ways to implement this. Let's explain some:

The oldie one:

    var kingOfTheSevenKingdomsPrototype = {
        born: function(boyName) {
            this.name = boyName
        },
        getKing: function() {
            return this.name;
        }
    };

    function kingOfTheNorth(name) {
        function K() {};
        K.prototype = kingOfTheSevenKingdomsPrototype;

        var k = new K();
        k.killHimNow = function() {
            console.log("Ok, it's dead. It was a king in the north. What do you expect?");
        }

        k.born(name);
        return k;
    }

    var king = kingOfTheNorth("Ned Stark");
    console.log("Who is the king of the north. It's " + king.getKing());
    king.killHimNow();        
        
I think it's pretty unclear. All this prototype, with this new K() and all. But since ECMAScript 5 we have an alternative. And its `gooood`:

    var kingOfTheSevenKingdomsPrototype = {
        getKing: function() {
            return this.name;
        }
    };

    var kingOfTheNorth = Object.create(
            kingOfTheSevenKingdomsPrototype, {
                'name': {
                    value: 'Ned Stark',
                    enumerable: true
                },
                killHimNow: {
                    value: function() {
                        console.log("Ok, it's dead. It was a king in the north. What do you expect?");
                    },
                    enumerable: true
                }
            }
    );

    console.log("Who is the king of the north. It's " + kingOfTheNorth.getKing());
    console.log(kingOfTheNorth);
    kingOfTheNorth.killHimNow();


Ok, what happened here? It's a bit difficult to explain if you don't know anything about ECMASCRIPT 5, but it's simly enough: kingOfTheSevenKingdoms is an object and kingOfTheNorth is a "copy" of the object with two properties: name and killHimNow. When we call `kingOfTheNorth.getKing()` we are using the "father" function. When we call `kingOfTheNorth.killHimNow();` we are using the "child" one. And if we want all ECMASCRIPT 5:

   var kingOfTheSevenKingdomsPrototype = Object.create(Object.prototype, {
        name: {
            value: "",
            enumerable: true
        },
        getKing: {
            value: function() {
                return this.name;
            },
            enumerable: true
        },

    })

    var kingOfTheNorth = Object.create(
            kingOfTheSevenKingdomsPrototype, {
                'name': {
                    value: 'Ned Stark',
                    enumerable: true
                },
                killHimNow: {
                    value: function() {
                        console.log("Ok, it's dead. It was a king in the north. What do you expect?");
                    },
                    enumerable: true
                }
            }
    );

    console.log("Who is the king of the north. It's " + kingOfTheNorth.getKing());
    kingOfTheNorth.killHimNow();
    
That's the way to do it :)
    
Ok, I know this is a little hard but try to understand it: it simplifies a lot how we can do things in inheritance. And remember: the functions are in the same position of memory and this is really awesome when you have dozens of objects!      
        