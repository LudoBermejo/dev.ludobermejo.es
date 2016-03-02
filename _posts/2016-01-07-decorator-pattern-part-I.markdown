---
layout: post
title:  "Decorator pattern (part I)"
date:   2016-01-07 19:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/decoratorpartone.jpg"
---

Decorators! Oh my, this is getting serious. Ok, first is first: you know OOP has changed in the last decade. At first all was about classes, inheritance and so. But step by step the patterns has changed the way of doing things, not only on Javascript but on every aspect of the development process. And Decorator pattern is one of the most successful patterns that brings the new paradigm (where 'new' is really old in terms of development cycles, but you know what I mean).
 
 Ok but, what's this `Decorator Pattern`. Oh well, in a nutshell: it's a design pattern that adds behaviour to an existing object in execution time. So you don't need to declare the class with that functionality; you just need to `add` the functionality to the object.
 
 But, what's that all about? What's the thing this decorator will remove from usual OOP? Ok, now we must learn the old ways of doing things. We need to talk about `subclassing`. Let's do an example:
 
 
    function Animal(name, family) {
        this.name = name;
        this.family = family;
    }
    
    var bear = new Animal("Panda", "Bear");

Ok, that's easy, right? Just an animal. But what if we want to be a... kung-fu panda?
     
     
    // Ok this is really old shit, but come on, this is the simplest and oldest way to do it

    function Animal(name, family) {
        this.name = name;
        this.family = family;
    }

    function KungfuBear(name, family, moves) {
        Animal.call(this,name, family);
        this.moves = moves;
    }

    KungfuBear.prototype = Object.create(Animal.prototype);

    console.log(KungfuBear);
    var kungfuPanda = new KungfuBear("Panda", "Bear", [ 
        "Twelve Impossible Moves", "Nerve attack"
    ]);
    console.log(kungfuPanda);

Ok, that's a subclass. Now let's find the problem. So imagine, I don't know, you want another type of bear. Just, I don't know... ok, `Yogui bear`. What's then? Do we create a subclass called "cartoon bear", and then another subclass for the kungfuBear. That's will do the trick, yeah, but imagine doing that in a full project... pain! PAIINNNN!
   
So what's about decorators? First, let me explain that the following code is the simplest way we can use a decorator. It's ugly, it's expensive and in some ways it doesn't make any sense. But you need to be a worm before you can be a human. Unless you are believer, of course.
   
        // Ok, let's do the stuff
    
        function Animal(name, family) {
            this.name = name;
            this.family = family;
        }
    
        var kungfuPanda = new Animal("Panda", "Bear");
        var yoguiBear   = new Animal("Grizzly", "Bear");
        var realBear   = new Animal("Grizzly", "Bear");
    
        // Ey, but yogui and realbear are the same!!
    
        kungfuPanda.setTypeCartoon = yoguiBear.setTypeCartoon = function() {
            this.isCartoon = true;
        }
    
        kungfuPanda.setTypeCartoon();
        yoguiBear.setTypeCartoon();
    
        // And what about the kugfu moves?
    
        kungfuPanda.setMoves = function(moves) {
            this.moves =  moves;
        }
    
        kungfuPanda.setMoves(["Twelve Impossible Moves", "Nerve attack"]);
    
        console.log(kungfuPanda);
        console.log(yoguiBear);
        console.log(realBear);
    
        /** This is the result:
         *
         *  Kungfu panda
         * "{"name":"Panda","family":"Bear","isCartoon":true,"moves":
            ["Twelve Impossible Moves","Nerve attack"]}"
         *
         *  Yogui bear
         * "{"name":"Grizzly","family":"Bear","isCartoon":true}"
         *
         * Real bear
         *
         "{"name":"Grizzly","family":"Bear"}"
         */
         
Ok, you can imagine what I did: I have added some behaviours to an object in realtime. As I said it's not the best example in the world, but it can show you some things. 

Now, let's add another explanation to our pool.
          
      function StarWarsGuy(name, origin) {
          this.name = name;
          this.origin = origin;
  
          this.getImportance = function() {
              return 0;
          }
  
      }
  
      function HasASpaceShip(starWarsGuy) {
          var importance = starWarsGuy.getImportance();
  
          starWarsGuy.getImportance = function() {
              return importance + 10;
          }
      }
  
      function isAJedi(starWarsGuy) {
          var importance = starWarsGuy.getImportance();
  
          starWarsGuy.getImportance = function() {
              return importance + 20;
          }
      }
  
      function isASkywalker(starWarsGuy) {
          var importance = starWarsGuy.getImportance();
  
          starWarsGuy.getImportance = function() {
              return importance + 50;
          }
      }
  
      var luke = new StarWarsGuy("Luke","Tattoine");
  
      HasASpaceShip(luke);
      isAJedi(luke);
      isASkywalker(luke);
  
      console.log(luke.getImportance());
      
      // Yeah, you know... 80, it's less important that Anakin!
      
      
Did you see what happened here? We modify the object by adding layers of complexity in the same object. We are overriding the getImportance but in a certain way: it respects the old values.
      
      
Ok, enough for one day! Next day we will start doing more complex things with this operators. For now... have fun!                       
              
       
       
       
       
    
    
