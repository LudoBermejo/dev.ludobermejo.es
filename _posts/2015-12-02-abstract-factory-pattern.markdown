---
layout: post
title:  "Abstract factory pattern"
date:   2015-12-02 19:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/mixin_pattern.jpg"
---

Wow! Almost a month without posting in here! Maybe I'm too busy with refactoring our ugly, ugly portal :( Yeah, I know there are worst things but it takes a lot of time that we could use, I don't know, maybe using a good combination between language & framework (PHP and Fatfree are not the best combo a developer can use)
 
But enough of this shit! Today we will talk about `Abstract factory`, a derivation of Factory Pattern (I talked about it in [this post](http://dev.ludobermejo.es/patterns/2015/10/21/factory-pattern.html)).
 
But what is this `Abstract Factory` thing about? Well, we use it when we want a system that must work with multiple type of objects. If you remember the "ticket to starwars example" of the [previous post](http://dev.ludobermejo.es/patterns/2015/10/21/factory-pattern.html), you know it only works in cinemas. But, what if I want a class that can work with cinemas, concerts and wrestling? Then you MUST use this pattern! (or maybe not, you know, it's only an idea :))
  
Ok, I think this can be explain with a little example. Let's try it!
 
   
    // Ok, let's define all three type of tickets
    function CinemaTicket( options ) {
        // some defaults
        this.movie = options.movie || "Birdman";
        this.canChangeSeats = options.canChangeSeats || false;
        this.row = options.row || "more or less centered";
        this.seat = options.seat || "crick creator";
        this.childs = options.childs || "yes, with the mobile full charged";
    }

    // A constructor for define what a regular ticket is
    function ConcertTicket( options ) {
        this.artist = options.artist || "Skrillex";
        this.date = options.date || "Only on christmas";
        this.where = options.where || "Tokyo stadium";
    }

    // A constructor for define what a premium ticket is
    function WrestlingTicket( options ) {
        // some defaults
        this.row = options.row || "more or less centered";
        this.seat = options.seat || "crick creator";
        this.combat = options.combat || "All stars";
    }

    // Ok, now we create an abstract class
    var AbstractTickerBuyerFactory = (function() {
        var typeOfTickets = {};

        return {

            // We can add the type of tickets we want to support
            addTypeOfTicket: function(name,classOfTicket) {
                typeOfTickets[name] = classOfTicket;
            },

            // And now the boring get ticket things
            getTicket: function(type, specs) {
                var Ticket = typeOfTickets[type];
                if(Ticket) return new Ticket(specs);
                return null
            }
        }
    })();

    // Ok, let's tregister the tickets
    AbstractTickerBuyerFactory.addTypeOfTicket("cinema", CinemaTicket);
    AbstractTickerBuyerFactory.addTypeOfTicket("concert", ConcertTicket);
    AbstractTickerBuyerFactory.addTypeOfTicket("wrestling", WrestlingTicket);

    // And know let's create some tickets:
    var giveMeACinemaTicket = AbstractTickerBuyerFactory.getTicket("cinema", {
        movie: "Star wars VIII",
        childs: "No way"
    })
    console.log(JSON.stringify(giveMeACinemaTicket));
    // returns {"movie":"Star wars VIII","canChangeSeats":false,"row":"more or less centered","seat":"crick creator","childs":"No way"}

    var giveMeAConcertTicket = AbstractTickerBuyerFactory.getTicket("concert", {
    })
    console.log(JSON.stringify(giveMeAConcertTicket));
    // returns {"artist":"Skrillex","date":"Only on christmas","where":"Tokyo stadium"}

    var giveMeAWrestlingTicket = AbstractTickerBuyerFactory.getTicket("wrestling", {
        combat: "Resurrected Ultimate warrior resurrected VS Old boy Hulk Hogan"
    })
    console.log(JSON.stringify(giveMeAWrestlingTicket));
    // returns "row":"more or less centered","seat":"crick creator","combat":"Last warrior resurrected VS Old boy Hulk Hogan"}

Easy, right? We just add the type of object we need, and we do it in execution time. Sweet! This is great use of Factory. But remember: the class must do only one thing. This kind of pattern is great but you can go to the "multiple behaviours by multiple type of objects". Just say no! It's a bad practice and it would convert your code in hell :)
