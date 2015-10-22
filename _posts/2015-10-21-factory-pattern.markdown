---
layout: post
title:  "Factory pattern"
date:   2015-10-21 19:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/factory_pattern.jpg"
---

Factory pattern! This is one of the favourites of (Markel Arizaga)[https://markelarizaga.wordpress.com/]: he always wants to include it in every module we use! And usually he is right: we use this pattern a lot in Starzplay.
  
  But, what is this all about? Well, this pattern is used to create objects without knowing the exact class of object we are creating. And how it do that? By using an interface that allows subclasses to use the class they want to instanciate.
  
  Confused? Yeah, me too. I'll try to explain it: imagine your boyfriend wants you to buy a ticket for the premiere of Star Wars (good luck with that!). So he doesn't care if that ticket comes from the cinema, from a friend or from internet. He only wants the ticket, that's all! So you are the interface that needs to find the better way to get the ticket, not your boyfriend.
  
  Let's see it in code:
  
        // A constructor for define what a cheap ticket is
        function CheapTicket( options ) {
    
            // some defaults
            this.price = options.price || 10;
            this.cinema = options.cinema || "Full as hell cinema";
            this.canChangeSeats = options.canChangeSeats || false;
            this.row = options.row || "more or less centered";
            this.seat = options.seat || "crick creator";
            this.childs = options.childs || "yes, with the mobile full charged";
        }
    
        // A constructor for define what a regular ticket is
        function RegularTicket( options ) {
    
            // some defaults
            this.price = options.price || 15;
            this.cinema = options.cinema || "Full but comfortable cinema";
            this.canChangeSeats = options.canChangeSeats || true;
            this.row = options.row || "more or less centered";
            this.seat = options.seat || "not as good as your girlfriend expects";
            this.childs = options.childs || "yes but they are quiet (for now)";
        }
    
        // A constructor for define what a premium ticket is
        function PremiumTicket( options ) {
    
            // some defaults
            this.price = options.price || 20;
            this.cinema = options.cinema || "'Is anyone there?' cinema";
            this.canChangeSeats = options.canChangeSeats || true;
            this.row = options.row || "centered";
            this.seat = options.seat || "centered";
            this.childs = options.childs || "we eat them for breakfast";
        }
    
        // Ey, that's you!
        function YouAsABuyerOfTicketsFactory() {}
    
        // Yes, the cheap is the first
        YouAsABuyerOfTicketsFactory.prototype.ticketClass = CheapTicket;
    
        // Ok let's buy it.
        YouAsABuyerOfTicketsFactory.prototype.buyTicket = function(options) {
            switch(options.typeOfCinema) {
                case "CheapTicket": this.ticketClass = CheapTicket; break;
                case "RegularTicket": this.ticketClass = RegularTicket; break;
                case "PremiumTicket": this.ticketClass = PremiumTicket; break;
            }
            return new this.ticketClass( options );
        }
    
        // Right we'll start
    
        var canIBuyASuperPremiumTicket = new YouAsABuyerOfTicketsFactory().buyTicket({
            typeOfCinema: "PremiumTicket",
            price: 50
        })
    
        console.log("Can i buy the ticket in this cinema?");
        console.log(canIBuyASuperPremiumTicket);
        console.log("Are you kidding me? Who is going to pay " + canIBuyASuperPremiumTicket.price  +" bucks for a movie?")
    
        var canIBuyRegularTicket = new YouAsABuyerOfTicketsFactory().buyTicket({
            typeOfCinema: "RegularTicket",
            seat: "in china"
        })
    
        console.log("Can i buy the ticket in this cinema?");
        console.log(canIBuyRegularTicket);
        console.log("No way I don't want to see the movie " + canIBuyRegularTicket.seat)
    
        var canIBuyCheapTicket = new YouAsABuyerOfTicketsFactory().buyTicket({
            typeOfCinema: "CheapTicket",
            row: "you can see the pixels"
        })
    
        console.log("Can i buy the ticket in this cinema?");
        console.log(canIBuyCheapTicket);
        console.log("No way I don't want to go to a cinema in which " +canIBuyCheapTicket.row);
    
        var canIBuyRegularTicket2 = new YouAsABuyerOfTicketsFactory().buyTicket({
            typeOfCinema: "RegularTicket",
            canChangeSeats: false
        })
    
        console.log("Can i buy the ticket in this cinema?");
        console.log(canIBuyRegularTicket2);
        console.log("Ok, boy, we can't change seats but... let's try it");
        
Ok, I think you can get the idea. It's a really good pattern, right? Why the hell are not going to use it everywhere?

Well, the problem with this pattern is that it can be used as a complex solution for really easy situations. You know, like use Angular or React for everything. We could use another pattern, the Abstract Factory one. But this pattern will be explained in the next article :)