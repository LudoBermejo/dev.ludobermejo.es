---
layout: post
title:  "Strategy pattern"
date:   2016-02-20 13:00:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/singleton-pattern.jpg"
---

`Strategy pattern` is a interesting but barely used pattern. It allows us to define multiple algorithms and make them interchangeable. So the "client" always call the same function and it receives the same result, but with different codes. 
 
 As always, it is more complicated explain it by regular words that show you the code. So today's I'm explaining to you three methods of open a door. The Gandalf's style, the Rambo's style and the Neo style. Let's do it:
 
    // So we have a door closed
 
    var Door = function() {
 
        var character;
        return {
            isClosed: true,
            character: character,
            whoIsInFrontOfMe: whoIsInFrontOfMe,
            openMe: openMe
        }
 
 
        function whoIsInFrontOfMe(c) {
            character = c;
         }
 
        function openMe() {
            if(character) {
                return isClosed =  character.openADoor();
            } else {
                return "Emptyness can't open the door"
            }
        }
 
    }
 
    // And we have three characters
 
     function Gandalf() {
         return {
             openADoor: openADoor
         }
 
         function openADoor() {
             return "Speak 'friend' and enter";
         }
     }
 
    function Rambo() {
        return {
            openADoor: openADoor
        }
 
        function openADoor() {
            return "Blow the f*cking door with an explosive arrow";
        }
    }
 
    function Neo() {
        return {
            openADoor: openADoor
        }
 
        function openADoor() {
            return "There is no door to open";
        }
    }
 
    // And now, create three doors and open them with the three characters
 
     var door1 = new Door();
     var door2 = new Door();
     var door3 = new Door();
 
     door1.whoIsInFrontOfMe( new Gandalf() );
     door2.whoIsInFrontOfMe( new Rambo() );
     door3.whoIsInFrontOfMe( new Neo() );
 
     console.log(door1.openMe())
     // Returns Speak 'friend' and enter
     console.log(door2.openMe())
     // Returns Blow the f*cking door with an explosive arrow
     console.log(door3.openMe())
     // Returns There is no door to open

Easy enough! But powerful. If you can use wisely this pattern you will simplify the reading of your code. And this is essential when you work in a complex project.