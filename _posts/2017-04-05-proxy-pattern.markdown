---
layout: post
title:  "Proxy pattern"
date:   2017-04-05 10:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/memento_pattern.jpg"
---

Proxy pattern! I must confess I have only used this pattern twice, both of them in the beginning of times, in ActionScript. Yeah, I was one of this silly boys that worked with Adobe Flash. I did some cool things but it makes no sense to remember that. What it's interesting is the reason I used this pattern.

But, what is this `Proxy patterns` thing? Well, a proxy object is only an object that, you know, "proxies" another object. You don't have access to the original object but to the proxy one, and the proxy calls the original object and return the pertinent data.

And, what can we do with this? Ok, we usually have three basic uses (and they are repeated in every book and blog that talk about this pattern):

__To delay the initialization of the object (Virtual proxy)__

Let's imagine you have an huge object that must do a high number of calculations when it is created. Maybe it's not your object but a third party one. You could use a proxy and create it instead of the original object. In this way you only will create the original object when you need it. 

So... well... I suppose we can get this kind of problem but I never have saw this in Javascript. Usually javascript objects are not as complicated, but if we go to the server side of the development, maybe we can find examples. For know, you can check this silly demo here; lets imagine you want to create a list of _The Quiz_ powers. The Quiz is one character of the Morrison's Doom Patrol enemies, the Brotherhood of Dada and... ok, forget about it, imagine a supervillain that has all the powers you have not thought. And you are Robotman, a robot... man, and you want to think all of them with a software. Imagine the amount of calculations you need to do: you only wants to do them if you meet with _The Quiz_, right? 

So first of all, we need to think a power list. To do that, we choose random words based on alphabet and put them together to create a power.

    var PowersList = function() {

        var powers = [];
        var possibleLetters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        var maxNumberOfLetters = 7;

        function createWord() {
            var text = "";
            for( var i=0; i < 7; i++ )
                text += possibleLetters.charAt(Math.floor(Math.random() * possibleLetters.length));
            return text;
        }

        function createPower() {
            var power = "";
            var totalWords = Math.floor(Math.random()*6)+1;
            console.log(totalWords);
            for(var i=0;i<=totalWords;i++) {
                power += createWord() + " " ;
            }
            return power;
        }

        function thinkOnPowers(n) {
            for(var i=0;i<=n;i++) {
                powers.push("The quiz will not have " + createPower() + " power<br>");
            }
        }
        
        function drawPowers() {
            console.log(powers); 
        }
        thinkOnPowers(5000);
    };

    var powerList = new PowersList();


Ok, now you can imagine that think in 5000 powers it's a little problematic if you don't meet The Quiz. So let's do a proxy:


    var PowerListProxy = function() {
    
        function IHaveMeetTheQuiz() {
        }
    
    }

__To simplify the way we access to an API (Remote proxy)__

More used 

