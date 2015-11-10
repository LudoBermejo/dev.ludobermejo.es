---
layout: post
title:  "Mixin pattern"
date:   2015-11-11 19:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/factory_pattern.jpg"
---

Mixins! Love that name, you now? Mixins are something I love in Javascript development: the inclusion of external ideas to our not-so-poor-day by day language. In other languages, mixins are used as classes that provide extra functionality to subclasses. You can use more that one mixin to inherit your class so you can use multiple functionality for different parent classes. But in Javascript we don't have multiple inheritance so it needs a hacky solution. Not very pretty but it's a way to understand a IMHO a better pattern, the Decorator. 
   
   But, how can we use mixins in our code? Ok, let's go:
   
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

As you can see, we can add multiple properties and functions to an object just by using this mixins. But as I said it's not the most understable way to do it. So let's wait to the explanation of decorator pattern! 

