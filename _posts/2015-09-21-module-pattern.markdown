---
layout: post
title:  "Module pattern"
date:   2015-09-21 18:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/houseOfThrones.jpg"
---

This is possible the most famous design pattern between javascript developers and it's used in every framework we found every week. Most of us has used this pattern even without knowing it, just by copying another example of "how to do a todo list" (tm)

But it is so useful, really? Well, maybe fifteen years ago it was a little too overcomplicated. But right now, with all the stuff we have (javascript clients, javascript servers, javascripts webapps, javascript soloapps, etc), I think it's the most easy way to keep your app in line. Unless you wanna make some kind of spaghetti skynet, you know.
   
So then, what's the module pattern? Well, first of all, to understand this pattern we need to talk about one thing first, the
 
 # Object literal
 
 This is just a way to create objects by using parameters separated by comma and encapsulated in curly braces, just like this:
 
    var discworld = {
            numberOfGiantElephants: 4,
            numberOfHugeTurtles: 1,
            colorOfMagic: "octarine",
            aWonderfulWorld: function() {
              console.log("We have " + this.numberOfGiantElephants + " elephants over " + this.numberOfHugeTurtles + " all over the " + this.colorOfMagic);
            },
            peopleSay: function (say) {
              console.log("People: "+say);
            },
            deathSay: function (say) {
              console.log("Death: "+this.peopleSay(say.toUpperCase()));
            }
        };
      
    discworld.aWonderfulWorld();  
    discworld.peopleSay("Day-o");  
    discworld.peopleSay("daaaaay-o");
    discworld.deathSay("Daylight come and me wan' go home");
    
See? It was easy. With `Object literal` you can encapsulate data and behaviour within and object, so this object has his own space to work.
      
But the sad thing is that everything in this object is public. And what if you don't want to have all the properties listed in your object? Well, now here it is the
      
# Module pattern
      
With this pattern you can use public and private properties and functions, just like you were using some "big brother" language. Well, not really, but it's a start.
      
But, how can things can be private? That's an excelent question! To do that we need to use `closures`. A `closure` is a way to wrap all private and public methods but only showing the public ones. It's easy enough to learn it and powerful enough to use it. And the best of this is that there are a myriad of ways to achieve this. Let's see it in an example:
        
        var discWorld = (function() {
          var thievesGuildPassword = "Whale";
          
          return {
          
            thievesGuildPasswordMaybe: "No way",
            
            whatsThePassword: function (pass) {
              if(pass === thievesGuildPassword) {
                console.log("You may pass");
              } else {
                console.log("We will remember you... no, not really");
              }
            }
          }
        });
        
        console.log(discWorld.thievesGuildPassword);
        console.log(discWorld().thievesGuildPassword);
        console.log(discWorld().thievesGuildPasswordMaybe);
        discWorld().whatsThePassword("Melon");
                        
        discWorld().whatsThePassword("Whale");

So look at this. We cannot reach this `thievesGuildPassword` property but we can reach the public functions and the `thievesGuildPasswordMaybe` one. Please take a look at the (function() { }) thing. That's encapsulation.

Cool, right? And it could be better. Let's make another example:

        var discWorld = (function() {
          var thievesGuildPassword = "Whale";
          
          function bribe() {
            console.log("Ok you may pass but it's your funeral");
          }

          
          return {
          
            thievesGuildPasswordMaybe: "No way",

            iHaveALittleSomethingForYou: bribe,
            
            whatsThePassword: function (pass) {
              if(pass === thievesGuildPassword) {
                console.log("You may pass");
              } else {
                console.log("We will remember you... no, not really");
              }
            }
          }
        });
        
        console.log(discWorld.thievesGuildPassword);
        console.log(discWorld().thievesGuildPassword);
        console.log(discWorld().thievesGuildPasswordMaybe);
        discWorld().whatsThePassword("Melon");
        discWorld().whatsThePassword("Whale");
        discWorld().iHaveALittleSomethingForYou();
  
         
Did you see what's happened? We can create private functions, and we can use them from the public methods. So we can have private methods and private properties in an object. It's not because security but clearness; we can extract the structure of an object and we only see what we need to use, and not all the stuff we have. And this is awesome...

... or maybe not? Ok, the use of private methods is good but what's about the unit testing? Because you know, if we are using private functions, how can we get them to make some tests?
 
 In a short way: you can't. This is the worst of this pattern: you can't use unit testing to test private methods. It's a shame, it's a fucking problem. There are some workarounds like not using private methods but public ones with an underscore, just like _thisIsAPrivateMethod but... well... this is not as good as the pattern itself.
  
So what's the solution? Well, there are smartish guys over there thinking about everything. You might found a solution in [the internet](https://www.youtube.com/watch?v=iDbyYGrswtg), or you can wait to our next lesson.

Your choice!!
          

         