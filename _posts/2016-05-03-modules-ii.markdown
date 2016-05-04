---
layout: post
title:  "Modules II: Functions"
date:   2016-04-18 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/modules1.jpg"
---

In our [previous post](http://dev.ludobermejo.es/es2015/2016/04/18/modules-i.html), I started the talk about modules. We know now how we can export variables in ES6. But, what about functions? Can't we export functions in the module? Of course we can! Let's see how we can do it:
 
 Ok, let's check the code:
 
### index.html

     <html>
     <head>
         <script src="bower_components/traceur/traceur.min.js"></script>
         <script src="bower_components/es6-module-loader/dist/es6-module-loader-dev.js"></script>
     </head>
     
     <body>
     
         <script>
             System.import("js/base.js")
         </script>
     
     </body>
     </html>
     
### js/module_values.js

    let yourRealName = "Batman";
    let yourFacadeName = "Bruce Wayne";
    
    export function whatsYourName() {
        return yourFacadeName;
    };   
     
### js/base.js

     import { whatsYourName, ImAFriend } from 'js/module_values.js';
     console.log(whatsYourName());
    
This will return this
    
    Bruce Wayne
        
Ok, so, what's happened here? I have to local variables, yourRealName and yourFacadeName, and I only export a function that returns the facade name. Then I only need to call it. 
     
Easy enought, right? But let's see more examples.

### js/module_values.js
    
    import { whatsYourName, ImAFriend } from 'js/module_values.js';
    console.log(whatsYourName());
    ImAFriend();
    console.log(whatsYourName());
     
### js/base.js
    
    let yourRealName = "Batman";
    let yourFachadeName = "Bruce Wayne";
    
    export function whatsYourName() {
        return yourFachadeName;
    };
    
    export function ImAFriend() {
        console.log("But I'm your friend Jason. What's your real name?")
        whatsYourName = () => yourRealName;
    };
    
Will return
    
    Bruce Wayne
    But I'm your friend Jason. What's your real name?
    Batman
    
So, what happened? We first call to the function whatsYourName. Then, we call a second function that changes the first function variable. So when we call again the whatsYourName, we use the changed variable, not the original one. And that's important because this proves that the exports of the module are not new objects, but the same that are created in the module. You can change a variable in the "father" module and it will be changed in the "son" module.    