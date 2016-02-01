---
layout: post
title:  "Higher-order functions (Functional programming I)"
date:   2016-02-01 19:30:00
author: Ludo Bermejo
categories: Functional 
tags:	functional 
cover:  "assets/decoratorpartone.jpg"
---

Ok, I know, I know, I must continue with the decorator pattern. But in the last few days I'm learning about functional programming and I want to share with you my first tests.

So, first is first, what's `functional programming`? It's a very valid question and a hard like hell one to answer to. But let's attend to the book:

`Functional Programming` is a style of programming that uses functions, not objects or procedures, as the fundamental parts of the program.

Yes, you're right. You have not f*cking idea of what I'm talking about. I'm a bad teacher and right now I'm still learning how to solve some problems I had with this approach of programming. In the next articles I will try to understand all the possibilities of this thing, by doing some examples. I will be painfully slow because I have so many things to do right now... but trust me: at the end of this journey I will understand completely this new paradigm. And I hope I can help you to understand it too.
   
So, first is first, let's talk about the `Higher-order functions`.
   
`Higher-order functions` are functions that operate on other functions, either by taking them as arguments or by returning them.
   
"Oh waitwaitwaitwait", you might say, "Is this your idea of help? Speak in tongues?". Ok, let's try this one:
   
A `Higher-order function` is a function that takes as argument another function. It is not a very complete definition but definitely is more clear. 
   
# First example Higher-order functions: the FILTER function 
   
   Ok, lets figure we have an array like this one:
   
         var gameOfThronesCharacters = [
            { name: "John Stark", house: "Unknown"},
            { name: "Catelyn Stark", house: "Tully"},
            { name: "Jorah Mormont", house: "Mormont"},
            { name: "Jaime Lannister", house: "Lannister"},
            { name: "Tyrion Lannister", house: "Lannister"},
            { name: "Sansa Stark", house: "Stark"},
            { name: "Robert Baratheon", house: "Baratheon"},
            { name: "Benjen Stark", house: "Stark"},
            { name: "Cersei Lannister", house: "Lannister"},
            { name: "Aria Stark", house: "Stark"}
        ]

  We want to get all the characters that are from the Lannister house. So, in the old times, we should do something like this:
  
        function doItInTheAllWay(house) {

              var arrayResult = [];
              for(var i=0;i<=gameOfThronesCharacters.length-1;i++) {
                  if(gameOfThronesCharacters[i].house == house) {
                      arrayResult.push(gameOfThronesCharacters[i])
                  }
              }
              return arrayResult;
          }

          console.log(JSON.stringify(doItInTheAllWay("Lannister"), null, 2));

  Ok, we put this on code and we will get this result:
  
        [
            {
              "name": "Jaime Lannister",
              "house": "Lannister"
            },
            {
              "name": "Tyrion Lannister",
              "house": "Lannister"
            },
            {
              "name": "Cersei Lannister",
              "house": "Lannister"
            }
          ]
          
  Perfect thing, right? But... do you know we can do that in a veeeery short amount of code? Now look at this:
            
 
        function doItInTheProperWay(house) {
            return gameOfThronesCharacters.filter(function(character) {
                return character.house === "Lannister";
            });
        }
        
        console.log(JSON.stringify(doItInTheProperWay("Lannister"), null, 2));
 
  Now let's have a look at the result:

      [
          {
            "name": "Jaime Lannister",
            "house": "Lannister"
          },
          {
            "name": "Tyrion Lannister",
            "house": "Lannister"
          },
          {
            "name": "Cersei Lannister",
            "house": "Lannister"
          }
        ]
        
  WHATTTT? Oh yes, `filter function` it's an `Higher-order functions`. Filter will loop thought each item in the array. And it will pass each item to the callback function (just the anonymous function we created with the comparison). This callback function must return true or false. After the iteration this will return a new filtered array. Great right? Ok, but let's try a new thing. 
        
        function isLannister(character) {
            return character.house === "Lannister";
        }
        
        function doItInTheProperWay(house) {
            return gameOfThronesCharacters.filter(isLannister)
        }
        
        console.log(JSON.stringify(doItInTheProperWay("Lannister"), null, 2));

 Ah, this is more clear, right? Maybe not so great, but much more clear. 
 
# Second example Higher-order functions: the FILTER function
 
Ok now I will show you how you can get all the names of the caracters. We'll do it in the traditional way and then we will use the `map function`. 
 
So first we have an array:
 
      var gameOfThronesCharacters = [
             { name: "John Stark", house: "Unknown"},
             { name: "Catelyn Stark", house: "Tully"},
             { name: "Jorah Mormont", house: "Mormont"},
             { name: "Jaime Lannister", house: "Lannister"},
             { name: "Tyrion Lannister", house: "Lannister"},
             { name: "Sansa Stark", house: "Stark"},
             { name: "Robert Baratheon", house: "Baratheon"},
             { name: "Benjen Stark", house: "Stark"},
             { name: "Cersei Lannister", house: "Lannister"},
             { name: "Aria Stark", house: "Stark"}
         ]

And all we want is to get the name of the characters, so I'm making this:

    var names = [];
    for(var i in gameOfThronesCharacters) {
        names.push(gameOfThronesCharacters[i].name);
    }
    console.log(JSON.stringify(names, null, 2));

And we get the names:

    [
      "John Stark",
      "Catelyn Stark",
      "Jorah Mormont",
      "Jaime Lannister",
      "Tyrion Lannister",
      "Sansa Stark",
      "Robert Baratheon",
      "Benjen Stark",
      "Cersei Lannister",
      "Aria Stark"
    ]
    
But we can do it much better! Just look at this:

    var names = gameOfThronesCharacters.map(function(character) {
        return character.name
    });
    console.log(JSON.stringify(names, null, 2));
    

And we get, too,  the names:

    [
      "John Stark",
      "Catelyn Stark",
      "Jorah Mormont",
      "Jaime Lannister",
      "Tyrion Lannister",
      "Sansa Stark",
      "Robert Baratheon",
      "Benjen Stark",
      "Cersei Lannister",
      "Aria Stark"
    ]    
    
So, what's map? Map returns not a true/false, but an object that will be included in the result. For example, we can return an array of strings:

    var names = gameOfThronesCharacters.map(function(character) {
        return character.name + " is from the " + character.family + "'s House";
    });
    console.log(JSON.stringify(names, null, 2));

Will return
    
    [
      "John Stark is from the Unknown's House",
      "Catelyn Stark is from the Tully's House",
      "Jorah Mormont is from the Mormont's House",
      "Jaime Lannister is from the Lannister's House",
      "Tyrion Lannister is from the Lannister's House",
      "Sansa Stark is from the Stark's House",
      "Robert Baratheon is from the Baratheon's House",
      "Benjen Stark is from the Stark's House",
      "Cersei Lannister is from the Lannister's House",
      "Aria Stark is from the Stark's House"
    ]
    
    
# Third example Higher-order functions: the REDUCE function    

So what's REDUCE? It's some kind of multi-purpose tool we can use to make any transformation to a list. You could use REDUCE to code MAP or FILTER, if you want (but I don't recommend this).
 
Let's check this example. I want to sum the num of childs of the houses of game of thrones. So we are using this:

    var gameOfThronesCharacters = [
            { house: "Stark", childs: 5},
            { house: "Lannister", childs: 3 },
            { house: "Baratheon", childs: 3 },
            { house: "Tully" , childs: 4}
    ]

Ok, now let's count the childs with the old stuff:

    var sum = 0;
    for(var i in gameOfThronesCharacters) {
        sum += gameOfThronesCharacters[i].childs || 0;
    }
    console.log(sum);
    
And we get a 15. Great but... can we use a high level function? Let's see it:
    
    var sum = gameOfThronesCharacters.reduce(function(sum, house) {
        return sum + house.childs;
    },0);

    console.log(sum);
    
What? 15 again? Ok what have happened. Well, reduce accept two parameters: the "sum" one is a object that is stored in every iteration. The house is the current object in the iteration. And the "0" is used as the original value of the object that will be stored in every iteration.    

Ok, let's see another example, boring but useful. This example will give me the families with more that 3 childs:

    var gameOfThronesCharacters = [
        { house: "Stark", childs: 5},
        { house: "Lannister", childs: 3 },
        { house: "Baratheon", childs: 3 },
        { house: "Tully" , childs: 4}
    ]
    var housesWithMoreThat3Childs = gameOfThronesCharacters.reduce(function(moreThan3, house) {
        if(house.childs >3)
            moreThan3.push(house)
        return moreThan3;
    },[]);

    console.log(JSON.stringify(housesWithMoreThat3Childs, null, 2));

See? Easy and potent.

# CONCLUSIONS

Ok, those are very powerful features you can use right now. But better than that, they give you a more precisely idea of what we can do with this `Higher-order functions`.

We will talk about this in the future. For know, have a nice code day!