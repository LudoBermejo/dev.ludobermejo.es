---
layout: post
title:  "Flyweight pattern"
date:   2016-02-29 10:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/flyweight_pattern.jpg"
---

The Flyweight pattern starts with a very specific hypothesis: you can save memory by reusing identical data in multiple objects. This idea could be strange in a world with gbs of RAM; but in the last few years we are using ever growing interfaces, with more and more objects. Think on a mail webapp, like gmail. It has an interface with lots of rows, and each one of them has elements in common: colors, behaviours, etc. You can store every duplicated element or make a "common element object" and points it to the rows. That's what we do with the flyweight pattern: we make objects that share common data between different objects. Those objects must be instantiated only one time, and must be not modified during the execution, so you can guarantee that object will be the same for every other object that needs it.

Now, let's show an example. Imagine you want to make a Saint Seiya knights' database. Every knight shares info with other knights, but they also have custom info. Let's look at this:

     /**
       First of all, we define what is our common data layer.
   
       In this case, we create a common data layer with three elements:
   
       Affiliation
       Class
       Rank
      */
   
      function FlyweightSaintSeiyaKnight(affiliation, classOfKnight, rank  ) {
          var affiliation = affiliation;
          var classOfKnight = classOfKnight;
          var rank = rank;
          return {
              get: get
          };
   
          function get() {
              return "\n   Affiliation: " + affiliation
                   + "\n   Class of knight:" + classOfKnight
                   + "\n   Rank:" + rank
          }
      }
   
      /**
       * Now we declare a factory that would create a list of commons data.
       *
       * It will create a new object for every different
       * Affiliation + class + rank
       * 
       * @type {Function}
       */
      var FlyweightSaintSeiyaKnightFactory = ( function() {
   
          var saintSeiyaKnights = [];
   
          return {
              get: get
          }
   
          function get(affiliation, classOfKnight, rank  ) {
              if(!saintSeiyaKnights[affiliation + "-"
                  + classOfKnight + "-" + rank]) {
                       saintSeiyaKnights[affiliation + "-"
                       + classOfKnight + "-" + rank] = 
                           new FlyweightSaintSeiyaKnight (
                               affiliation, classOfKnight, rank
                       )
              }
              return saintSeiyaKnights[affiliation
                                       + "-"
                                       + classOfKnight
                                       + "-" + rank];
          }
   
      })
   
      /**
       * Now we define a knight. It must have:
       *
       * @param name
       * @param cloth
       * @param affiliation
       * @param classOfKnight
       * @param rank
       *
       * and it can return all data (personal and common)
       *
       * @returns {{get: get}}
       * @constructor
       */
      var SaintSeiyaKnight = function (
              name, cloth, affiliation, classOfKnight, rank
      ) {
          var commonData = FlyweightSaintSeiyaKnightFactory()
                           .get(affiliation, classOfKnight, rank);
          var cloth = cloth;
          var name = name;
   
          return {
              get: get
          }
   
          function get() {
              return "Knight " + name
                      + " wears " + cloth
                      + ".\nCommon data " + commonData.get()
          }
      }
   
      /**
       *
       * Finally we create a list that can add a new knight,
       * and can return all the data of all knights
       *
       * @returns {{add: add, get: get}}
       * @constructor
       */
      var SaintSeiyaKnightList = function() {
          var listOfKnights = [];
          return {
              add: add,
              get: get
          }
          function add(name, cloth, affiliation, classOfKnight, rank) {
              listOfKnights[name] = new SaintSeiyaKnight(
                      name, cloth, affiliation, classOfKnight, rank
              );
          }
   
          function get() {
              for(var i in listOfKnights) {
                  console.log(listOfKnights[i].get()+ "\n\n");
              }
          }
      }
   
   
      // So let's start
      var list = new SaintSeiyaKnightList();
   
      // Bronze
      list.add("Dragon Shiryū", "Dragon Cloth",
              "Athena", "Saints", "Bronze Saints");
      list.add("Phoenix Ikki", "Phoenix Cloth",
              "Athena", "Saints", "Bronze Saints");
   
      // Silver
      list.add("Crane Yuzuriha", "Crane Cloth",
              "Athena", "Saints", "Silver Saints");
      list.add("Eagle Marin", "Eagle Cloth",
              "Athena", "Saints", "Silver Saints");
   
      // Gold
      list.add("Libra Dohko", "Libra Cloth",
              "Athena", "Saints", "Gold Saints");
      list.add("Aries Mū", "Aries Cloth",
              "Athena", "Saints", "Gold Saints");
   
      // Hades terrestrial
      list.add("Deep Niobe", "Deep Surplice",
              "Hades", "Specters", "Terrestrial Star Specters");
      list.add("Papillon Myu", "Papillon Surplice",
              "Hades", "Specters", "Terrestrial Star Specters");
   
      // Hades celestial
      list.add("Griffon Minos", "Griffon Surplice",
              "Hades", "Specters", "Celestial Star Specters");
      list.add("Garuda Aiacos", "Garuda Surplice",
              "Hades", "Specters", "Celestial Star Specters");
   
      // Let's see what we get:
       list.get();
   
      /**
       * It returns
   
       Knight Dragon ShiryÅ« wears Dragon Cloth.
       Common data
       Affiliation: Athena
       Class of knight:Saints
       Rank:Bronze Saints
   
   
       Knight Phoenix Ikki wears Phoenix Cloth.
       Common data
       Affiliation: Athena
       Class of knight:Saints
       Rank:Bronze Saints
   
   
       Knight Crane Yuzuriha wears Crane Cloth.
       Common data
       Affiliation: Athena
       Class of knight:Saints
       Rank:Silver Saints
   
   
       Knight Eagle Marin wears Eagle Cloth.
       Common data
       Affiliation: Athena
       Class of knight:Saints
       Rank:Silver Saints
   
   
       Knight Libra Dohko wears Libra Cloth.
       Common data
       Affiliation: Athena
       Class of knight:Saints
       Rank:Gold Saints
   
   
       Knight Aries MÅ« wears Aries Cloth.
       Common data
       Affiliation: Athena
       Class of knight:Saints
       Rank:Gold Saints
   
   
       Knight Deep Niobe wears Deep Surplice.
       Common data
       Affiliation: Hades
       Class of knight:Specters
       Rank:Terrestrial Star Specters
   
   
       Knight Papillon Myu wears Papillon Surplice.
       Common data
       Affiliation: Hades
       Class of knight:Specters
       Rank:Terrestrial Star Specters
   
   
       Knight Griffon Minos wears Griffon Surplice.
       Common data
       Affiliation: Hades
       Class of knight:Specters
       Rank:Celestial Star Specters
   
   
       Knight Garuda Aiacos wears Garuda Surplice.
       Common data
       Affiliation: Hades
       Class of knight:Specters
       Rank:Celestial Star Specters
        */
 
 Now I hope you understand: there is only one object with Affiliation, Class and Rank, so it will not consume all our memory. Imagine this with 200 knights... see what I'm talking about?
 
 Sweeeeeet :)