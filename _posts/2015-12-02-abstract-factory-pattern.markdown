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
 
   
            // First we define our hero builder
            var Hero = function(params) {
                this.name = params.name || "Nonamed";
                this.gender = params.gender || "Female";
                this.whoAmI = function() {
                    console.log("I am " + this.name + ", a " + this.gender)
                }
            }
        
            // Then we define another hero builder
            var AnotherHero = function(params) {
                this.name = params.name || "Nonamed";
                this.gender = params.gender || "Female";
                this.whoAmI = function() {
                    console.log("I am " + this.name + ", a " + this.gender)
                }
            }
        
            // We have a mixin with a fantasy template
            var FantasyHero = function() {}
            FantasyHero.prototype = {
                equipement: function() {
                    console.log("I have a sword")
                },
                special: function() {
                    console.log("I use magic");
                }
            }
        
            // We have a mixin with a scifi template
            var ScifiHero = function() {}
            ScifiHero.prototype = {
                equipement: function() {
                    console.log("I have a laser gun")
                },
                special: function() {
                    console.log("I use technology magic");
                }
            }
        
            function addMixin(GetClass, mixin) {
        
                var methods = [];
                // if we have more arguments
                if(arguments[2]) {
                    methods = Array.prototype.slice.call(arguments).splice(2,2);
        
                } else {
                    methods = Object.getOwnPropertyNames(mixin.prototype);
                }
        
                for(var i=0;i<=methods.length-1;i++) {
                    GetClass.prototype[methods[i]] = mixin.prototype[methods[i]];
                }
            }
        
            // Now we add the first mixin to the first hero class builder with two functions
            addMixin(Hero, FantasyHero, "equipement", "special");
        
            // Ok let's create a new hero!
        
            var heroFemale = new Hero({name: "Elisa", gender: "female"})
            console.log(heroFemale);
            heroFemale.whoAmI();
            heroFemale.equipement();
            heroFemale.special();
        
            // And now we add the second mixin to the second hero class builder with all the functions
            addMixin(AnotherHero, ScifiHero);
        
            // and create the second hero
            var heroMale = new AnotherHero({name: "Enrico", gender: "male"})
            console.log(heroMale);
            heroMale.whoAmI();
            heroMale.equipement();
            heroMale.special();

Easy, right? We just add the type of object we need, and we do it in execution time. Sweet! This is great use of Factory. But remember: the class must do only one thing. This kind of pattern is great but you can go to the "multiple behaviours by multiple type of objects". Just say no! It's a bad practice and it would convert your code in hell :)
