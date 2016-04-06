---
layout: post
title:  "Block scoping, let and const"
date:   2016-04-06 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/letandconstandblock.jpg"
---

Hi all! I have started to learn ES2015 or ES6, the new & shinny version of Javascript. It has lots of improvements and I'll use this blog to write about them for future reference. If you find this useful, then I will be glad :)

Now, let's go. Today I will talk about two new keywords for declare variables (let and const) and the new scope blocks.
 
# LET
 
 Let's imagine this example:
 
    var IWantToKnowTheSecretIdentityOf = "Batman";
     
    if( IWantToKnowTheSecretIdentityOf === "Batman") {
        var response = "Bruce Wayne";
    } else {
        var response = "Superman";
    }
    
    console.log(response) 
    
What would you see? Yes, you will see:
    
    Bruce Wayne
    
Now imagine let. Stealing the definition from mozilla.org, *let allows you to declare variables that are limited in scope to the block, statement, or expression on which it is used. This is unlike the var keyword, which defines a variable globally, or locally to an entire function regardless of block scope.*
    
Now, what is a block. Let me use the same code that before:

 var IWantToKnowTheSecretIdentityOf = "Batman";
    
    let response = "55"; 
    if( IWantToKnowTheSecretIdentityOf === "Batman") {
        let response = "Bruce Wayne";
    } else {
        let response = "Superman";
    }
    
    console.log(response);

 What do you think we will get?     
 
    Uncaught ReferenceError: response is not defined
    
What? Why? That's because we have defined the let variable in a block. The "if" statement has two blocks, one for the positive and other for the rest. A `block` in ES2015, as far I know, is anything that is between curly braces or in specific commands.
    
Now let's see another example    
     
     let response = "No one"
     var IWantToKnowTheSecretIdentityOf = "Batman";
     if( IWantToKnowTheSecretIdentityOf === "Batman") {
         let response = "Bruce Wayne";
     } else {
         let response = "Superman";
     }
 
     console.log(response);
         
What do you think you can get? This is the result

    No one
        
Again, we have changed the response variable in the scope of the if, not in the parent scope. So first value is maintained. It will be the same of this:
        
    let IWantToKnowTheSecretIdentityOf = "Batman";
    {
        let IWantToKnowTheSecretIdentityOf = "Superman";
    }
    
    console.log(IWantToKnowTheSecretIdentityOf);
        
Now this is the result
        
    Batman
        
Again, we are using the let variable in a specific block, so it will only have this value in this block.

A new example:

    {
        let IWantToKnowTheSecretIdentityOf = "Superman";
    }
    console.log(IWantToKnowTheSecretIdentityOf);

Will give us this result

    Uncaught ReferenceError: IWantToKnowTheSecretIdentityOf is not defined
        
exactly for the same reason: we defined this let variable in a block statement.
        
Now let's go with a tricky one:

    function putBatmanInTheName() {
        whoIsBruceWayne = "Batman";
    }
    
    let whoIsBruceWayne = "I don't know";
    putBatmanInTheName();
    console.log(whoIsBruceWayne);
        
What do you think we will get? "I don't know"? "Batman"? "undefined"? Let's check it:
       
    Batman
    
Ok but, why is that? We have defined the function **before** the let. Of course you are right, but remember that you have executed this function **after** the declaration. And the javascript engine works on execution.

Now a tricky one:

    let numberOFSupermanPowers = 0;
    
    for(let numberOFSupermanPowers=1;numberOFSupermanPowers<=7;numberOFSupermanPowers++) {
        console.log("Increasing numberOfSupermanPowers to " + numberOFSupermanPowers)
    }
    
    console.log("Final number of superman powers " + numberOFSupermanPowers);

What will be the final number of superman powers? Let's execute and see:

    Increasing numberOfSupermanPowers to 1
    Increasing numberOfSupermanPowers to 2
    Increasing numberOfSupermanPowers to 3
    Increasing numberOfSupermanPowers to 4
    Increasing numberOfSupermanPowers to 5
    Increasing numberOfSupermanPowers to 6
    Increasing numberOfSupermanPowers to 7
    
    Final number of superman powers 0
    
What? Why is that? Well, that's because the FOR statement is used like a block. Weird? No, wonderful. Imagine this:

    
      for(let numberOFSupermanPowers=1;numberOFSupermanPowers<=7;numberOFSupermanPowers++) {
             console.log("Increasing numberOfSupermanPowers to " + numberOFSupermanPowers)
         }
         
      console.log("Final number of superman powers " + numberOFSupermanPowers);

What will be the final number of superman powers now?
     
     Increasing numberOfSupermanPowers to 1
     Increasing numberOfSupermanPowers to 2
     Increasing numberOfSupermanPowers to 3
     Increasing numberOfSupermanPowers to 4
     Increasing numberOfSupermanPowers to 5
     Increasing numberOfSupermanPowers to 6
     Increasing numberOfSupermanPowers to 7
     
     Uncaught ReferenceError: numberOFSupermanPowers is not defined

Yes! This variable will be undefined outside the for block! This is **awesome**! And why is awesome? Because of the typical problem of the closures. Let's do an usual problem:

    var users = [
        { name: "ludo" },
        { name: "john" },
        { name: "jaume" }]
    
    for(var i=0;i<=users.length-1;i++) {
    
        // let's imagine we call an ajax load. It will delay the assignation 100 milliseconds
        function dataOfUserLoaded() {
            setTimeout(function() {
                users[i].id = i;
            },100)
        }
    
        dataOfUserLoaded();
    }
    
    // And now we wait 500 milliseconds to check the result
    setTimeout(function() {
        console.log(JSON.stringify(users));
    },500)
        
What is the result of this?
        
    Uncaught TypeError: Cannot set property 'id' of undefined(anonymous function) @ prueba.html:16
    [{"name":"ludo"},{"name":"john"},{"name":"jaume"}]
    
And why is that? Because when the program ends the loading, the variable is 3. That's because the for is ended. 
 
But now... what if we use the let?
 
    var users = [
        { name: "ludo" },
        { name: "john" },
        { name: "jaume" }]

    for(let i=0;i<=users.length-1;i++) {

        // let's imagine we call an ajax load. It will delay the assignation 100 milliseconds
        function dataOfUserLoaded() {
            setTimeout(function() {
                users[i].id = i;
            },100)
        }

        dataOfUserLoaded();
    }

    // And now we wait 500 milliseconds to check the result
    setTimeout(function() {
        console.log(JSON.stringify(users));
    },500)

The result is:
 
    [{"name":"ludo","id":0},{"name":"john","id":1},{"name":"jaume","id":2}]
    
And why is that? Because the closure of the let variable is a block. I repeat: this is **awesome**!           

# CONST

Ok, `const` is very similar to `let` but it will define a constant. So for example, you could do this:

    let whoIsClackKent;
    
but you can't do this:
    
    const whoIsClarkKent;
    
Because you will get this error:
    
    Uncaught SyntaxError: Missing initializer in const declaratio
    
You always need to declare a value for the const. And once the value is used, you can't change it:
    
    const whoIsClarkKent = "Superman";
    whoIsClarkKent = "Batman";
    
You wil get:
    
    Uncaught TypeError: Assignment to constant variable
    
And no, you can't do this:
    
    const whoIsClarkKent = "Batman";
    const whoIsClarkKent = "Superman";
    
Because you will get another error:
    
    Uncaught SyntaxError: Identifier 'whoIsClarkKent' has already been declared
    
But what about blocks? Can we do this?
    
    const whoIsClarkKent = "Superman";
    if(whoIsClarkKent  == "Superman")
    {
       const whoIsClarkKent = "Batman";
       console.log(whoIsClarkKent);
    }

Oh, yes, you could do that and it will show:
         
    Batman
    
But check this:
    
    const whoIsClarkKent = "Superman";
    if(whoIsClarkKent  == "Superman")
    {
       const whoIsClarkKent = "Batman";
       console.log(whoIsClarkKent);
    }
    console.log("Really? Can you tell me who clark kent is?");
    console.log("ok he is " + whoIsClarkKent);

The result will be:

    Batman
    Really? Can you tell me who clark kent is?
    ok he is Superman
    
And why is that? Because you have defined the const in a block, and when you are outside the block, you will maintain the original value of the const
    
# Conclusions    
   
This functionality is awesome. It will solve a lot of problems, specially to newcomers to the javascript's world. I want to use them ASAP :) 
    
But for now, enough of this. I think I have showed enough examples to understand the `let`, the `const` and the `block scoping`. I hope it will be useful for you, guys.      
    