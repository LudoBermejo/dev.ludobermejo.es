---
layout: post
title:  "traceur & module loader boilerplate"
date:   2016-04-17 10:30:00
author: Ludo Bermejo
categories: ES2015 
tags:	es2015
cover:  "assets/rest_and_spread.jpg"
---

This will be a "just configuration" post. We are going to talk about some improvements in ES6 that has not been implemented yet in the browsers. So we need to use some polyfills to support them in browsers. 

To support you in an easy way, I have created a boiler plate for es6. Please check this repository [es6-boilerplate](https://github.com/LudoBermejo/es6-boilerplate).
 
Now let me explain you what packages we are going to use:

# Traceur

*Extract from the [original page of traceur](https://github.com/google/traceur-compiler)*

Traceur is a JavaScript.next-to-JavaScript-of-today compiler that allows you to use features from the future today. Traceur supports ES6 as well as some experimental ES.next features.

So yes, Traceur supports ES6 and ES.next in browsers. Of course it's not perfect, but it is usable in production environments and, frankly, it's awesome.

# es6-module-loader

*Extract from the [original page of es6-module-loader](https://github.com/ModuleLoader/es6-module-loader) 

Dynamically loads ES6 modules in browsers and NodeJS with support for loading existing and custom module formats through loader hooks.

So with this polyfill we can use the new module feature of ES6.

In the following weeks I will add some improvements to the project. But, right now, you can follow my examples just by cloning the project and using it.




