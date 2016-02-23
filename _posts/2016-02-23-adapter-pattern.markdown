---
layout: post
title:  "Adapter pattern"
date:   2016-02-23 10:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/strategy.jpg"
---

Another semi-unknown pattern (but still very used if you work with JQUERY) is the `Adapter pattern`. It is used to translate the properties and methods of one object (the interface) to another object. Why would we do that? Imagine you have an huge and complex system, and imagine that you want to update it. You could revamp the code completely, but that's a lot of effort. You can take the `Adapter pattern` way too, and do it step by step until you complete the code. And what you need to do? A progressively changing class? No, you can make an Adapter that allows the new class understand the old class. 

And how can you do that? I'm glad you asked it; let's play with the code:

   // So let's create the t800

    var T800 = function() {

        return {
            openADoor: openADoor,
            breakAGlass: breakAGlass,
            pursueSarahConnor: pursueSarahConnor
        }

        function openADoor() {
            console.log("T800 open the doors by crushing cars into them");
        }

        function breakAGlass() {
            console.log("T800 can break a glass by pushing it with his bare hands");
        }

        function pursueSarahConnor() {
            console.log("T800 can follow Sarah Connor by car and motorcycle");
        }
    }

    // So t800 can do lot's of things:

    var t800 = new T800();

    t800.openADoor();
    // T800 open the doors by crushing cars into them
    t800.breakAGlass();
    // T800 can break a glass by pushing it with his bare hands
    t800.pursueSarahConnor();
    // T800 can follow Sarah Connor by car and motorcycle

Ok, we have a new and shinny t800. Sarah Connor, be careful!... or maybe not because...
    
<iframe width="420" height="315" src="https://www.youtube.com/embed/2KeniFoiT-0" frameborder="0" allowfullscreen></iframe>

Ok, that didn't work. So Skynet, what we can do? Oh, yeah, we have this thing, the t1000. It does a LOT of things:

    // So let's create the t1000
    
    var T1000 = function() {
    
       return {
           changeHandIntoThing: changeHandIntoThing,
           runWithPowerLegs: runWithPowerLegs,
           changeArmIntoKnife: changeArmIntoKnife
       }
    
       function changeHandIntoThing(thing) {
           return "T1000 transforms his hand into " + thing;
       }
    
       function runWithPowerLegs() {
           return "T1000 runs at 100mph";
       }
    
       function changeArmIntoKnife() {
           return "T1000 transforms his hand into knife";
       }
    }
    
    // So t1000 can do much more things that T800
    
    var t1000 = new T1000();
    
    console.log(t1000.changeHandIntoThing("mace"));
    // T1000 transforms his hand into mace
    console.log(t1000.runWithPowerLegs());
    // T1000 runs at 100mph
    console.log(t1000.changeArmIntoKnife());
    // T1000 transforms his hand into knife


But, hey, to be honest, we use this man as plumber. You know he doesn't have the right interface and changing his orders... well, it's a lot of stress for the engineers. What can we do?

Well then, you can use the (tadaaaaa) `Adapter pattern`.

    function T1000Adapter() {
      var t1000 = new T1000();
    
      return {
          openADoor: openADoor,
          breakAGlass: breakAGlass,
          pursueSarahConnor: pursueSarahConnor
      };
    
    
      function openADoor() {
          return  t1000.changeHandIntoThing("key") + " and open the door";
      }
    
      function breakAGlass() {
          return t1000.changeArmIntoKnife() + " and slices the glass";
      }
    
      function pursueSarahConnor() {
          return t1000.runWithPowerLegs() + " and catch Sarah Connor"
      }
    }
    
    var t1000Adapter = new T1000Adapter();
    
    console.log(t1000Adapter.openADoor());
    // T1000 transforms his hand into key and open the door
    console.log(t1000Adapter.breakAGlass());
    // T1000 transforms his hand into knife and slices the glass
    console.log(t1000Adapter.pursueSarahConnor());
    // T1000 runs at 100mph and catch Sarah Connor
   
So, as you can see, the t1000 can do exactly the same that t800 but without polluting the class. That's what `Adapter pattern` is for.

And yes, I know, the example is a little crappy but how many tutorials have a TERMINATOR as a example? 