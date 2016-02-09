---
layout: post
title:  "Singleton pattern"
date:   2016-02-09 13:00:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/singleton-pattern.jpg"
---

Oh my! I totally forgot this pattern, one of the most used in interviews :D

With this pattern you can reduce the amount of instances of a particular object. You will use only one. And this one instance is called singleton. This pattern was usually used in the past but now it's a little obsolete, cos in the present we a are using `Module pattern` and `Dependency injection` patterns in javascript. But I suppose it can be useful in some scenarios.

Now, how can we create a singleton object in javascript?


    var DarthVader = (function () {
        var instance;
        function create() {
            var random = Math.round(Math.random()*1000);
            function whatDoYouThinkAboutYoda() {
                return "it's a nasty greeny thing";
            }
            function whatDoYouThinkAboutLuke() {
                return "i'm his father";
            }
            function giveMeARandomNumber() {
                return random;
            }
            return {

                whatDoYouThinkAboutYoda: whatDoYouThinkAboutYoda,
                whatDoYouThinkAboutLuke: whatDoYouThinkAboutLuke,
                giveMeARandomNumber: giveMeARandomNumber
            }
        }

        return {
            getInstance: function () {
                if (!instance) {
                    instance = create();
                }
                return instance;
            }
        };
    })();



    var darthVader1 = DarthVader.getInstance();
    var darthVader2 = DarthVader.getInstance();

    console.log("YODA: " + darthVader1.whatDoYouThinkAboutYoda());
    console.log("LUKE: " + darthVader2.whatDoYouThinkAboutLuke());

    console.log("Darth 1, GIVE ME A RANDOM NUMBER: " + darthVader1.giveMeARandomNumber());
    console.log("Darth 2, GIVE ME A RANDOM NUMBER: " + darthVader1.giveMeARandomNumber());

Ok, this is the result of this script:

    YODA: it's a nasty greeny thing
    LUKE: i'm his father
    Darth 1, GIVE ME A RANDOM NUMBER: 229
    Darth 2, GIVE ME A RANDOM NUMBER: 229
    
As you can see, Darth 1 and Darth 2 returns exactly the same number. That's not because they are clones, but because they are exactly the same object!
     
TADAAAA!!!