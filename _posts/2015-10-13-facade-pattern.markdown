---
layout: post
title:  "Facade pattern"
date:   2015-10-13 22:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/prototype-pattern.jpg"
---

First time I read about this pattern I don't believe it. So, after all, I was doing `one thing` as expected. In the beginning of internet I worked for the two browsers: Internet explorer and Netscape. In those terrible times we need to hack every javascript call, not only for the different browsers but for the different versions of the same browsers, versions that popup every three months. We had an huge amount of lines of code just to select a DOM object and hide it. Because of the complexity of this calls, I started a library to "group" those complex calls in one only function (like `hide` or `remove`).
   
Well, that was a proper use of the Facade pattern.

The Facade pattern is just an interface that you use to "hide" the complex operations behind a simple call. I'm sure you have used it in jquery, just like this: 
 
    $("p .hide").show();
    
This method, `show` make the DOM object visible... in every browser, with or without `display: none` or `opacity 0`. It's a simplification so the user can just focus in what is important. It's maybe simple but it's very very useful. 
      
Now let's try a demo with people who can, or cannot die:


    var Human = function() {

        var isShooted = false;
        var isKilled = false;

        return {
            shootHim: shootHim,
            killHim: killHim
        }

        function shootHim() {
            console.log("Shooted in the body");
            isKilled = true;
        }

        function killHim() {
            if (isShooted) {
                isKilled = true;
            }
            return isKilled;
        }

    }


    var Zombie = function () {

        var isSlowed = false;
        var isDown = false;
        var hasHeadDamage = false;
        var isKilled = false;

        return {

            shootHimInTheBody: shootHimInTheBody,
            shootHimInTheKnee: shootHimInTheKnee,
            shootHimInTheHead: shootHimInTheHead,
            killHim: killHim
        }

        function shootHimInTheBody() {
            console.log("Shooted in the body");
            isSlowed = true;
        }

        function shootHimInTheKnee() {

            if (isSlowed) {
                console.log("Shooted in the knee");
                isDown = true;
            }
        }

        function shootHimInTheHead() {
            if (isDown) {
                console.log("Shooted in the head");
                hasHeadDamage = true;
            }
        }

        function killHim() {
            if (hasHeadDamage) {
                isKilled = true;
            }
            return isKilled;
        }
    }

    var Dracula = function () {


        var youAreInTheMansion = false;
        var youGoDownToTheBasement = false;
        var youOpenTheCoffin = false;
        var impaledWithAStake = false;
        var headCutted = false;
        var garlicInHisMouth = false;
        var isKilled = false;

        return {
            gotoTheMansion: gotoTheMansion,
            goDownTheBasement: goDownTheBasement,
            openTheCoffin: openTheCoffin,
            impaleHim: impaleHim,
            cutHisHead: cutHisHead,
            putGarlicInHisMouth: putGarlicInHisMouth,
            killHim: killHim
        }

        function gotoTheMansion() {
            console.log("You are in the mansion")
            youAreInTheMansion = true;
        }

        function goDownTheBasement() {
            if (youAreInTheMansion) {
                console.log("You are in the basement")
                youGoDownToTheBasement = true;
            }
        }

        function openTheCoffin() {
            if (youGoDownToTheBasement) {
                console.log("You open the coffin")
                youOpenTheCoffin = true;
            }
        }

        function impaleHim() {
            if (youOpenTheCoffin) {
                console.log("You impaled dracula with a stake")
                impaledWithAStake = true;
            }
        }

        function cutHisHead() {
            if (impaledWithAStake) {
                console.log("You cut the head of dracula")
                headCutted = true;
            }
        }

        function putGarlicInHisMouth() {
            if (headCutted) {
                console.log("You put garlic in his mouth")
                garlicInHisMouth = true;
            }
        }

        function killHim() {
            if (garlicInHisMouth) {
                isKilled = true;
            }

            return isKilled;
        }

    }

    console.info("KILLING A RANDOM GUY");
    var someRandomGuy = new Human();
    console.log("Can I kill him " + someRandomGuy.killHim());
    someRandomGuy.shootHim();
    console.log("Can I kill him now " + someRandomGuy.killHim());


    console.info("KILLING A RANDOM ZOMBIE");

    var someRandomZombie = new Zombie();
    console.log("Can I kill him " + someRandomZombie.killHim());
    someRandomZombie.shootHimInTheBody();
    console.log("Can I kill him " + someRandomZombie.killHim());
    someRandomZombie.shootHimInTheKnee();
    console.log("Can I kill him " + someRandomZombie.killHim());
    someRandomZombie.shootHimInTheHead();
    console.log("Can I kill him " + someRandomZombie.killHim());

    console.info("KILLING DRACULA");

    var dracula = new Dracula();
    console.log("Can I kill him " + dracula.killHim());
    dracula.gotoTheMansion();
    console.log("Can I kill him " + dracula.killHim());
    dracula.goDownTheBasement();
    console.log("Can I kill him " + dracula.killHim());
    dracula.openTheCoffin();
    console.log("Can I kill him " + dracula.killHim());
    dracula.impaleHim();
    console.log("Can I kill him " + dracula.killHim());
    dracula.cutHisHead();
    console.log("Can I kill him " + dracula.killHim());
    dracula.putGarlicInHisMouth();
    console.log("Can I kill him " + dracula.killHim());

As you can see everybody die... but some have more complexity that others. But what if we add this function: killDirectly with this different code:

    // For human
    function killDirectly() {
            shootHim();
            return killHim();
        }

    // For zombie
    function killDirectly() {
                shootHimInTheBody();
                shootHimInTheKnee();
                shootHimInTheHead();
                return killHim();
            }
            
    // For Dracula
    function killDirectly() {
            gotoTheMansion();
            goDownTheBasement();
            openTheCoffin();
            impaleHim();
            cutHisHead();
            putGarlicInHisMouth();
            return killHim();
        }                
    
Well, you can imagine. We can made a simple call (killDirectly) and kill those b*stards in only one order. I mean, I don't need to know how to do it, I just need to do the thing.
 
That's `Facade` :)

(you can see the complete code in here:)

    var Human = function() {

        var isShooted = false;
        var isKilled = false;

        return {
            shootHim: shootHim,
            killHim: killHim,
            killDirectly: killDirectly
        }

        function killDirectly() {
            shootHim();
            return killHim();
        }

        function shootHim() {
            console.log("Shooted in the body");
            isKilled = true;
        }

        function killHim() {
            if (isShooted) {
                isKilled = true;
            }
            return isKilled;
        }

    }


    var Zombie = function () {

        var isSlowed = false;
        var isDown = false;
        var hasHeadDamage = false;
        var isKilled = false;

        return {

            shootHimInTheBody: shootHimInTheBody,
            shootHimInTheKnee: shootHimInTheKnee,
            shootHimInTheHead: shootHimInTheHead,
            killHim: killHim,
            killDirectly: killDirectly
        }

        function killDirectly() {
            shootHimInTheBody();
            shootHimInTheKnee();
            shootHimInTheHead();
            return killHim();
        }

        function shootHimInTheBody() {
            console.log("Shooted in the body");
            isSlowed = true;
        }

        function shootHimInTheKnee() {

            if (isSlowed) {
                console.log("Shooted in the knee");
                isDown = true;
            }
        }

        function shootHimInTheHead() {
            if (isDown) {
                console.log("Shooted in the head");
                hasHeadDamage = true;
            }
        }

        function killHim() {
            if (hasHeadDamage) {
                isKilled = true;
            }
            return isKilled;
        }
    }

    var Dracula = function () {


        var youAreInTheMansion = false;
        var youGoDownToTheBasement = false;
        var youOpenTheCoffin = false;
        var impaledWithAStake = false;
        var headCutted = false;
        var garlicInHisMouth = false;
        var isKilled = false;

        return {
            gotoTheMansion: gotoTheMansion,
            goDownTheBasement: goDownTheBasement,
            openTheCoffin: openTheCoffin,
            impaleHim: impaleHim,
            cutHisHead: cutHisHead,
            putGarlicInHisMouth: putGarlicInHisMouth,
            killHim: killHim,
            killDirectly: killDirectly
        }

        function killDirectly() {
            gotoTheMansion();
            goDownTheBasement();
            openTheCoffin();
            impaleHim();
            cutHisHead();
            putGarlicInHisMouth();
            return killHim();
        }

        function gotoTheMansion() {
            console.log("You are in the mansion")
            youAreInTheMansion = true;
        }

        function goDownTheBasement() {
            if (youAreInTheMansion) {
                console.log("You are in the basement")
                youGoDownToTheBasement = true;
            }
        }

        function openTheCoffin() {
            if (youGoDownToTheBasement) {
                console.log("You open the coffin")
                youOpenTheCoffin = true;
            }
        }

        function impaleHim() {
            if (youOpenTheCoffin) {
                console.log("You impaled dracula with a stake")
                impaledWithAStake = true;
            }
        }

        function cutHisHead() {
            if (impaledWithAStake) {
                console.log("You cut the head of dracula")
                headCutted = true;
            }
        }

        function putGarlicInHisMouth() {
            if (headCutted) {
                console.log("You put garlic in his mouth")
                garlicInHisMouth = true;
            }
        }

        function killHim() {
            if (garlicInHisMouth) {
                isKilled = true;
            }

            return isKilled;
        }

    }

    console.info("KILLING A RANDOM GUY");
    var someRandomGuy = new Human();
    console.log("Can I kill him " + someRandomGuy.killHim());
    someRandomGuy.shootHim();
    console.log("Can I kill him now " + someRandomGuy.killHim());


    console.info("KILLING A RANDOM ZOMBIE");

    var someRandomZombie = new Zombie();
    console.log("Can I kill him " + someRandomZombie.killHim());
    someRandomZombie.shootHimInTheBody();
    console.log("Can I kill him " + someRandomZombie.killHim());
    someRandomZombie.shootHimInTheKnee();
    console.log("Can I kill him " + someRandomZombie.killHim());
    someRandomZombie.shootHimInTheHead();
    console.log("Can I kill him " + someRandomZombie.killHim());

    console.info("KILLING DRACULA");

    var dracula = new Dracula();
    console.log("Can I kill him " + dracula.killHim());
    dracula.gotoTheMansion();
    console.log("Can I kill him " + dracula.killHim());
    dracula.goDownTheBasement();
    console.log("Can I kill him " + dracula.killHim());
    dracula.openTheCoffin();
    console.log("Can I kill him " + dracula.killHim());
    dracula.impaleHim();
    console.log("Can I kill him " + dracula.killHim());
    dracula.cutHisHead();
    console.log("Can I kill him " + dracula.killHim());
    dracula.putGarlicInHisMouth();
    console.log("Can I kill him " + dracula.killHim());


    console.info("Now kill a human directly");
    someRandomGuy = new Human();
    console.log(someRandomGuy.killDirectly());

    console.info("Now kill a zombie directly");
    someRandomZombie = new Zombie();
    console.log(someRandomZombie.killDirectly());

    console.info("Now kill Dracula directly");
    dracula = new Dracula();
    console.log(dracula.killDirectly());

