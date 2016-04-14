---
layout: post
title:  "Destructuring"
date:   2016-04-16 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/rest_and_spread.jpg"
---

ES6 supports supports `destructuring`. With this feature we can extract values from data stored in arrays (including strings) and objects. 

How can we do that? Simple enough:

# Arrays

    let rankingOfQuidditchCupIn1992 = [
        'Ravenclaw',
        'Gryffindor',
        'Hufflepuff',
        'Slythering'
    ]

    let [ champion, subchampion ] = rankingOfQuidditchCupIn1992;

    console.log(`The champion was ${champion}`);
    console.log(`The subchampion was ${subchampion}`);
    
This will return:
    
    The champion was Ravenclaw
    The subchampion was Gryffindor
    
Cool, right? We can destructure an array in different variables. And we don't need to create this variables in the same line:
    
    let champion = 'Unknown';
    let subchampion = 'Unknown';

    let rankingOfQuidditchCupIn1992 = [
        'Ravenclaw',
        'Gryffindor',
        'Hufflepuff',
        'Slythering'
    ];

    [ champion, subchampion ] = rankingOfQuidditchCupIn1992;

    console.log(`The champion was ${champion}`);
    console.log(`The subchampion was ${subchampion}`);

This will return the same result:

    The champion was Ravenclaw
    The subchampion was Gryffindor
    
Now this is more interesting that you thing. Please check this code:
    
    let rankingOfQuidditchCupIn1992 = [
        'Ravenclaw',
        'Gryffindor',
        'Hufflepuff',
        'Slythering'
    ]

    let [ ,, third ] = rankingOfQuidditchCupIn1992;

    console.log(`The third winner was ${third}`);
        
It will return
    
    The third winner was Hufflepuff

So yes, you can choose what you want to store. Even you could use default parameters

    let rankingOfQuidditchCupIn1992 = [
        'Ravenclaw',
        ,
        'Hufflepuff',
        'Slythering'
    ];
    
    let [ champion = 'I don\'t know', subchampion = 'Gryffindor' ] = rankingOfQuidditchCupIn1992;
    
    console.log(`The champion was ${champion}`);
    console.log(`The subchampion was ${subchampion}`);

And you will get the result with the default parameters:

    The champion was Ravenclaw
    The subchampion was Gryffindor
    
Finally, you could use the rest operator, just like this:
    
    let rankingOfQuidditchCupIn1992 = [
       'Ravenclaw',
       'Gryffindor',
       'Hufflepuff',
       'Slythering'
    ]
    
    let [ winner, ...others ] = rankingOfQuidditchCupIn1992;
    
    console.log(`The winner was ${winner}`); 
    console.log(`The others were ${others}`);
    
Will return:
    
    The winner was Ravenclaw
    The others were Gryffindor,Hufflepuff,Slythering
    
# Objects

`Objects` works like arrays, but with small changes. Let's see them with examples

    let redDragonPowers =
    {
        'BreathWeapon': 'A red dragon has one type of breath weapon, a cone of fire.',
        'LocateObject': 'A juvenile or older red dragon can use this ability as the spell of the same name, once per day per age category.',
        'SpellLikeAbilities': '3/day—suggestion (old or older); 1/day—find the path (ancient or older), discern location (great wyrm).',
        'Skills': 'Appraise, Bluff, and Jump are considered class skills for red dragons.'
    }

    let { BreathWeapon, Skills } = redDragonPowers;

    console.log(`The breath weapon description is ${BreathWeapon}`);
    console.log(`The skills are ${Skills}`);

This will return:

    The breath weapon description is A red dragon has one type of breath weapon, a cone of fire.
    The skills are Appraise, Bluff, and Jump are considered class skills for red dragons.
    
As you can see, we can decide what properties we want to store. We don't need to use them all. Now let me show you another example:
    
    let redDragonPowers =
    {
        'BreathWeapon': 'A red dragon has one type of breath weapon, a cone of fire.',
        'LocateObject': 'A juvenile or older red dragon can use this ability as the spell of the same name, once per day per age category.',
        'SpellLikeAbilities': '3/day suggestion (old or older); 1/day find the path (ancient or older), discern location (great wyrm).',
        'Skills': 'Appraise, Bluff, and Jump are considered class skills for red dragons.'
    }

    let { BreathWeapon: principalWeapon , SpellLikeAbilities: customPowers } = redDragonPowers;

    console.log(`The breath weapon description is ${principalWeapon}`);
    console.log(`The skills are ${customPowers}`);
    
This will return:
    
    The breath weapon description is A red dragon has one type of breath weapon, a cone of fire.
    The skills are 3/day suggestion (old or older); 1/day find the path (ancient or older), discern location (great wyrm).
    
And now, I show you one problem you maybe could have. Let's check this code:

    let redDragonPowers =
    {
        'BreathWeapon': 'A red dragon has one type of breath weapon, a cone of fire.',
        'LocateObject': 'A juvenile or older red dragon can use this ability as the spell of the same name, once per day per age category.',
        'SpellLikeAbilities': '3/day suggestion (old or older); 1/day find the path (ancient or older), discern location (great wyrm).',
        'Skills': 'Appraise, Bluff, and Jump are considered class skills for red dragons.'
    }

    let principalWeapon, customPowers;

    { BreathWeapon: principalWeapon , SpellLikeAbilities: customPowers } = redDragonPowers;

    console.log(`The breath weapon description is ${principalWeapon}`);
    console.log(`The skills are ${customPowers}`);
    
Ok, easy. We have declared the principalWeapon and customPowers variables outside, and we are using them inside, just like the arrays. What can be wrong. Let's see the result:
    
    Uncaught SyntaxError: Unexpected token :
    
Sorry... what? What's the problem with this? Ok, problem are the curly bracers. Remember first post about ES6? About the blocks of code and the curly bracers? Yes, in this code Javascript is waiting for a block of code, not an object.
    
So, how can I avoid this problem. Well, you will not like it:


    let redDragonPowers =
    {
        'BreathWeapon': 'A red dragon has one type of breath weapon, a cone of fire.',
        'LocateObject': 'A juvenile or older red dragon can use this ability as the spell of the same name, once per day per age category.',
        'SpellLikeAbilities': '3/day suggestion (old or older); 1/day find the path (ancient or older), discern location (great wyrm).',
        'Skills': 'Appraise, Bluff, and Jump are considered class skills for red dragons.'
    }
    
    let principalWeapon, customPowers;
    
    ( 
        { BreathWeapon: principalWeapon , SpellLikeAbilities: customPowers } = redDragonPowers 
    );
    
    console.log(`The breath weapon description is ${principalWeapon}`);
    console.log(`The skills are ${customPowers}`);
    
    
See the parenthesis? Yes, you need to include them in order to use the custom external variable assignations. It's not the most clear thing to do... but it is what it is.
     
     
# Conclusions

What can I say? It's a cool feature and I think I could use it in some places, specially with filters and reduce. I hope I can think on a better example for it in the few months :)