---
layout: post
title:  "Revealing Module pattern"
date:   2015-09-24 18:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/discWorld.jpg"
---

So, if you remember last day, we talk about the [module pattern](http://dev.ludobermejo.es/patterns/2015/09/21/module-pattern.html). Now we will talk about a variant of this pattern: the `Revealing Module` pattern.
  
This pattern (created by Christian Heilmann in his [famous post](http://christianheilmann.com/2007/08/22/again-with-the-module-pattern-reveal-something-to-the-world/) takes the module pattern to a new consideration, by organizing the code into private functions to clarify the structure. So, if you remember last code:


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
  
The approx of Christian was to completely separate functions and variables for the public interface. So you never "attack" any property directly and every function has a private counterpart. Looking at the previous code:
         
         
      var DiscWorld = (function() {
          var thievesGuildPassword = "Whale";

          function bribe() {
              console.log("Ok you may pass but it's your funeral");
          }
          function getPassword() {
              return "No way"
          }
          function setPassword(newP) {
              thievesGuildPassword = newP;
              console.log("Really?");
          }
          function thePassword(pass) {

              if (pass === thievesGuildPassword) {
                  console.log("You may pass");
              } else {
                  console.log("We will remember you... no, not really");
              }
          }
          return {
              thievesGuildPasswordMaybe: getPassword,
              iHaveALittleSomethingForYou: bribe,
              whatsThePassword: thePassword,
              okGuysWeThisIsTheNewPassword: setPassword
          }
      });

      console.log(DiscWorld.thievesGuildPassword); // returns undefined

      var discWorld = new DiscWorld();

      console.log(discWorld.thievesGuildPassword); // returns undefined
      console.log(discWorld.thievesGuildPasswordMaybe()); // returns "no way"
      discWorld.whatsThePassword("Melon"); //log "We will remember you... no, not really"
      discWorld.whatsThePassword("Whale"); // log "You may pass"  
      discWorld.iHaveALittleSomethingForYou(); // log "Ok you may pass but it's your funeral"
      discWorld.okGuysWeThisIsTheNewPassword("Rincewind"); // log "Really?";
      discWorld.whatsThePassword("Whale"); // log "We will remember you... no, not really"
      discWorld.whatsThePassword("Rincewind"); // log "You may pass"
      
This module has some problems, as `Module Pattern`. When a private function uses a public function, this public function can't be overridedn, because the private function always will refer to the private function. Yes, I know, it's some kind of tongue-twister but I think it's mostly clear.