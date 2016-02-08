---
layout: post
title:  "Recursion (Functional programming III)"
date:   2016-02-08 13:00:00
author: Ludo Bermejo
categories: functional 
tags:	functional 
cover:  "assets/functional_programming_part_2.jpg"
---

Recursion! This is a familiar face for everyone who learned computing in the 90s. At that time recursion was like the hoy grail and all teachers said that you must use it whenever possible. First time I used it on a real project (about 1999) I get a memory error and about one hundred problems. Because, you know, Recursion was useful... only for a couple of things. After a few years recursion get the "don't use this" label and people forgot how to use it. And now it's coming back thanks to `functional programming`.

So, what's `Recursion`? Recursion is just a function that calls itself until some condition is reached. Then the function ends. That's all. It's very clear. Maybe it is not so clear where we can use it. I'll show you one example with nodes, trees and so on. First, let's look at this json:

    var gameOfThronesCharacters =  [
       { "name": "Tywin", "family": "Lannister", "childOf": null},
       { "name": "Tywin", "family": "Lannister", "childOf": null },
       { "name": "Kewan", "family": "Lannister", "childOf": null },
       { "name": "Cersei", "family": "Lannister", "childOf": "Tywin" },
       { "name": "Jaime", "family": "Lannister", "childOf": "Tywin" },
       { "name": "Tyrion", "family": "Lannister", "childOf": "Tywin" },
       { "name": "Lancel", "family": "Lannister", "childOf": "Kewan" },

       { "name": "Stannis", "family": "Baratheon", "childOf": null },
       { "name": "Edric", "family": "Baratheon", "childOf": "Stannis" },
       { "name": "Shireen ", "family": "Baratheon", "childOf": "Stannis" },
       { "name": "Petyr", "family": "Baratheon", "childOf": "Stannis" },
       { "name": "Tommard", "family": "Baratheon", "childOf": "Stannis" },
       { "name": "Renly", "family": "Baratheon", "childOf": null },
       { "name": "Robert", "family": "Baratheon", "childOf": null },
       { "name": "Joffrey", "family": "Baratheon","childOf": "Robert" },
       { "name": "Mircella", "family": "Baratheon","childOf": "Robert" },
       { "name": "Tommen", "family": "Baratheon","childOf": "Robert" },

       { "name": "Holter", "family": "Tully" },
       { "name": "Catelyn", "family": "Tully", "childOf": "Holter" },
       { "name": "Lysa", "family": "Tully", "childOf": "Holter" },
       { "name": "Edmure", "family": "Tully", "childOf": "Holter" },
       { "name": "Robin", "family": "Tully", "childOf": "Lysa" }
    ]
    
Now we want to make a family tree. What can we do to achive that? Well, we need to use `recursion`. Let's try it:

    // We get three arguments. The array, the name of family (if we want to filter for this, and the "childOf" for recursion
    var makeFamilyTree = function(data, family, childOf) {
    
        // We create a new object
        var node = {};
        
        // We filter by the childOf we pass as parameter. First time is null
        // We can also filter by the family parameter we sent.
        var filtered = gameOfThronesCharacters.filter(function(obj) {
            if(family != null) {
                return obj.childOf === childOf && obj.family === family
            } else {
                return obj.childOf === childOf
            }
        })

      
        for(var i in filtered) {
            
            if(!node[filtered[i].family]) node[filtered[i].family] = {}
            
            node[filtered[i].family][filtered[i].name] = makeFamilyTree(data, family, filtered[i].name)
        }
        return node;
        
       // Now we use a map to update the nodes  
       filtered.map(function(character) {
            // If it is a parent
            if(childOf === null) {
                // If the node doesn't have the family name, then create it
                if(!node[character.family]) node[character.family] = {};
                // Now we add the object found in the family name and in the parent name
                return node[character.family][character.name] = makeFamilyTree(data, family, character.name)
            } else {
                // If the node has parent, then put the data on the parent.
                return node[character.name] = makeFamilyTree(data, family, character.name)
            }
        });
    
        return node;
    }

    var result = makeFamilyTree(gameOfThronesCharacters, null, null);
    console.log(JSON.stringify(result,null,2))

    var result = makeFamilyTree(gameOfThronesCharacters, "Lannister", null);
    console.log(JSON.stringify(result,null,2))    
    
First console will return
    
    {
      "Lannister": {
        "Tywin": {
          "Cersei": {},
          "Jaime": {},
          "Tyrion": {}
        },
        "Kewan": {
          "Lancel": {}
        }
      },
      "Baratheon": {
        "Stannis": {
          "Edric": {},
          "Shireen ": {},
          "Petyr": {},
          "Tommard": {}
        },
        "Renly": {},
        "Robert": {
          "Joffrey": {},
          "Mircella": {},
          "Tommen": {}
        }
      }
    }
    
And second console will return
    
    {
      "Lannister": {
        "Tywin": {
          "Cersei": {},
          "Jaime": {},
          "Tyrion": {}
        },
        "Kewan": {
          "Lancel": {}
        }
      }
    }
    
See? It's not difficult to understand    