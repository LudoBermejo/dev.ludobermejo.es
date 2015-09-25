---
layout: post
title:  "Publish/Subscriber pattern"
date:   2015-09-22 18:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/pubsubpattern.jpg"
---

Also known as `Observer`, this pattern allows a `Subscriber` to listen to a `Publisher`. 
  
Ey... smart enought, isn`t it?
  
Ok, let's try again. So there is an Object (`Subscriber`) that wants to watch the responses of another object (`Publisher`). When the `Publisher` needs to notify the `Observers` about the response, it `Broadcast` a notificatio to every `Subscriber`. When the `Subscriber` is not interested any more in the responses, it can `Unregister` from the `Publisher`.

Better now? It's really no so hard, and I bet you are using this everytime you add a listener to a click. 

And what's the geniality of this? Mostly the idea of not coupling things. It's not about one object calling a method of other object... it's an object subscribing to another one. So any number of `Suscribers` can listen to any number of `Publishers` without knowing what is happening before and after the call.
 
Why is this helpful? Well, mostly because of the division. You can divide the objects in smaller ones so you can simplify your app 

And what about the "buts" of this approach? Well, the first is that then `Subscribers` don't know each others. And that the `Publisher` doesn't really know what is happening with the events. But that's another story.
 
Now, how can we build one of this patterns, in a `vanilla-javascript` style? Because every framework has his own Pub/Sub functions, but you will not learn how the things work only by using them. Let's create a quickly pub/sub implementation, just like this:

    // We can make it better, with closures, but we will keep this in focus
    function Publisher() {

        var events = []; // This array will store all the events
        var currentUUID = 0; // We can made it in other ways, but with this we can remove subscribers very fast
        return { // oh, yeah, revealing pattern
            subscribe: subscribe,
            unsubscribe: unsubscribe,
            publish: publish
        }

        /**
         * As you can imagine, this function subscribe a "event" wiwth a "function" by using the events array
         *
         * @param event
         * @param myFunc
         * @returns {number}
         */
        function subscribe(event, myFunc) {
            if(events[event] === undefined) {
                events[event] = [];
            }
            events[event].push( {
                uuid: ++currentUUID,
                call: myFunc
            });

            return currentUUID;
        }

        /**
         * Again this is pretty simple. We get the uuid we return in publish method and look for it until we found it
         * Then, we remove it from the subscribers
         *
         * @param uuid
         * @returns {boolean}
         */
        function unsubscribe(uuid) {
            for(var i in events) {
                for(var j=0;j<=events[i].length-1;j++) {
                    if(events[i][j].uuid === uuid) {
                        events[i].splice(j,1);
                        return true;
                    }
                }
            }
            return false;
        }


        /**
         * The publish itself. This function will call all the subscribers sending the event and the parameters received
         *
         * @param event
         * @param args
         * @returns {boolean}
         */
        function publish(event, args) {
            if(events[event]) {
                for(var i=0;i<=events[event].length-1;i++) {
                    events[event][i].call(event,args);
                }
            } else {
                return false;
            }
            return true;
        }
    }


    // So we create a function to test this functionality
    var testFunction = function(event, args) {
        console.log("I have raised by " + event + " with message", args)
    }

    // We create the publisher
    var publisher = new Publisher();

    // And three subscribers... two for Event1 and one for Event2
    var testSubscriptionEvent1First = publisher.subscribe("Event1", testFunction);
    var testSubscriptionEvent1Second = publisher.subscribe("Event1", testFunction);
    var testSubscriptionEvent2First = publisher.subscribe("Event2", testFunction);

    // We publish Event1, and console log must appear twice
    publisher.publish("Event1", "Call Event1");

    // We publish Event2, and console log must appear once
    publisher.publish("Event2", "Call Event2");

    // We remove one subscriber for Event1, so when we call event one, we only get one console.log
    publisher.unsubscribe(testSubscriptionEvent1First);
    publisher.publish("Event1", "Call Event1");

    // We remove the only subscriber for Event2, so when we call event two, we don't get anything in console.log
    publisher.unsubscribe(testSubscriptionEvent2First);
    publisher.publish("Event2", "Call Event2");

It was easy, right? We can use this pattern in multiple ways. We could use it to change some texts in a web page when we got some info, or we can change the page itself when an external error occurs. As they said, the limit is in your imagination!!  
