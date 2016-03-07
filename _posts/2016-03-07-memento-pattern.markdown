---
layout: post
title:  "Memento pattern"
date:   2016-03-07 10:30:00
author: Ludo Bermejo
categories: Patterns 
tags:	patterns 
cover:  "assets/memento_pattern.jpg"
---

Today we will see the `Memento pattern`, one partially-unknown pattern that we can use to solve some problems in modern developments.
 
 But, first, what's a Memento? Let's check the dict:

### _Memento: an object or item that serves to remind one of a person, past event, etc.; keepsake; souvenir._

That's a pretty good definition of what's this pattern about. With the `Memento pattern` we can provide a temporal storage for an object, so we can retrieve the saved state of the object if needed.
  
Most used scenario? History. We all love the Google Docs functionality of store *everything*, but mostly because we can *go back* whenever we want. I'm sure they are using some kind of `memento` pattern in his awesome software.
  
But enough chatting. Let's make a demo so you can understand how we can work with Mementos. Today we will see... let me think, ok, we'll see the different transformations of [Goku](http://dbzwarriors.com/saiyanforms.shtml), how he transforms and how hi came again to original form. 

Let's start!


    /**
     * So first we will create a Form object. It will be a little short but I hope
     * it will be enough to understand the problem
     */
    var GokuForm = (function (
            name,
            hairColor,
            size,
            hairUp,
            increasedMuscles
    ) {
        var characteristics = {};
        characteristics.name = name;
        characteristics.hairColor = hairColor;
        characteristics.size = size;
        characteristics.hairUp = hairUp;
        characteristics.increasedMuscles = increasedMuscles;
        return {
            getValue: getValue,
            getJSON: getJSON,
            setAllCharacteristics: setAllCharacteristics
        };

        // Get a specific characteristic
        function getValue(v) {
            if(characteristics[y])
                return characteristics[y]
            else
                return "What's that?";
        }

        // Return all the characteristics in string format
        function getJSON() {
            return JSON.stringify(characteristics);
        }

        // Convert string to characteristics
        function setAllCharacteristics(json) {
            characteristics = JSON.parse(json);
        }
    });

    /**
     * Now we will create a form storage. We will use it to store all the intermedial
     * forms Goku has transformed
     */

    var FormsStorage =  (function () {
        var forms = [];

        return {
            add: add,
            pop: pop,
            get: get
        };

        function add(characteristics) {
            forms.push(characteristics);
        }

        function pop() {
            return forms.pop();
        }

        function get() {
            return forms;
        }
    });


    /**
     * ANd now we create goku
     */
    var Goku = (function() {

        var actualForm = {};
        var formsStorage = new FormsStorage();

        return {
            getActualForm: getActualForm,
            changeForm: changeForm,
            restoreForm: restoreForm
        };

        function getActualForm() {
            return actualForm;
        }

        function changeForm(f) {
            actualForm = f;
            formsStorage.add(f.getJSON());
        }

        function restoreForm() {
            if(formsStorage.get().length)
            actualForm.setAllCharacteristics (
                    formsStorage.pop()
            )
        }

    })()


    /**
     * And now, the real thing.
     */
    console.log("Goku is walking in a forest");
    Goku.changeForm(
            GokuForm(
                    "Normal Goku",
                    "Black hair",
                    "Normal size",
                    false,
                    false
            )
    );
    console.log(Goku.getActualForm().getJSON());

    console.log("And he meets Piccolo. It's night now so he transforms in a girant gorilla!");
    Goku.changeForm(
            GokuForm(
                    "Oozaru",
                    "Black hair",
                    "Giant monkey",
                    false,
                    false
            )
    );
    console.log(Goku.getActualForm().getJSON());

    console.log("After he finish Piccollo, he goes to bed. But at dawn someone punches him. " +
            "It's Vegeta! They start fighting and Goku transforms again in a false Saiyan!");
    Goku.changeForm(
            GokuForm(
                    "False saiyan",
                    "Red hair",
                    "Normal size",
                    true,
                    true
            )
    );
    console.log(Goku.getActualForm().getJSON());

    console.log("After he finish Vegeta, he goes to have breakfast. But their beans " +
            "transform into Freeza!! Again, Goku transforms himself, " +
            "this time in Super Saiyan!");
    Goku.changeForm(
            GokuForm(
                    "Super saiyan",
                    "Blond hair",
                    "Normal size",
                    true,
                    true
            )
    );
    console.log(Goku.getActualForm().getJSON());

    console.log("All this combats and no breakfast at all. He goes to a bar " +
            "but the bartender doesn't want to feed him. That's because he is Cell! " +
            "Goku is a little angry so he transforms himself into Full-power Super Saiyan");
    Goku.changeForm(
            GokuForm(
                    "Full power super saiyan",
                    "Blond hair",
                    "Normal size",
                    true,
                    false
            )
    );
    console.log(Goku.getActualForm().getJSON());


    console.log("Sadly the fight has destroyed the bar so he cannot eat anything. " +
            "Furious, Goku transforms into a golden gorilla!");
    Goku.changeForm(
            GokuForm(
                    "Golden Oozaru",
                    "Blond hair",
                    "Giant monkey",
                    true,
                    false
            )
    );
    console.log(Goku.getActualForm().getJSON());
    console.log("Then he founds an orange-tree and he eat some oranges. " +
            "That makes he calm down and he fell asleep, still in golden form");
    Goku.restoreForm();
    console.log(Goku.getActualForm().getJSON());

    console.log("After that we fell asleep and he reverts its form to " +
            "Full power Super saiyan");
    Goku.restoreForm();
    console.log(Goku.getActualForm().getJSON());

    console.log("Now he wakes up hungry! Fortunately he found some fishes in the " +
            "river and he calms down to Super saiyan");
    Goku.restoreForm();
    console.log(Goku.getActualForm().getJSON());

    console.log("Mmm, too much light in this form. He goes down to False Super saiyan");
    Goku.restoreForm();
    console.log(Goku.getActualForm().getJSON());

    console.log("Oh, no, night again. Back to the gorilla form!");
    Goku.restoreForm();
    console.log(Goku.getActualForm().getJSON());

    console.log("The next morning he awakes normal again.");
    Goku.restoreForm();
    console.log(Goku.getActualForm().getJSON());



    


The final result is:

 Goku is walking in a forest

    {"name":"Normal Goku","hairColor":"Black hair",
    "size":"Normal size","hairUp":false,"increasedMuscles":false}

 And he meets Piccolo. It's night now so he transforms in a girant gorilla!

    {"name":"Oozaru","hairColor":"Black hair","
    size":"Giant monkey","hairUp":false,"increasedMuscles":false}

 After he finish Piccollo, he goes to bed. But at dawn someone punches him. It's Vegeta! They start fighting and Goku transforms again in a false Saiyan!

    {"name":"False saiyan","hairColor":"Red hair",
    "size":"Normal size","hairUp":true,"increasedMuscles":true}

 After he finish Vegeta, he goes to have breakfast. But their beans transform into Freeza!!Again, Goku transforms himself, this time in Super Saiyan!

    {"name":"Super saiyan","hairColor":"Blond hair",
    "size":"Normal size","hairUp":true,"increasedMuscles":true}

 All this combats and no breakfast at all. He goes to a bar but the bartender doesn't want to feed him. That's because he is Cell! Goku is a little angry so he transforms himself into Full-power Super Saiyan

    {"name":"Full power super saiyan","hairColor":"Blond hair",
    "size":"Normal size","hairUp":true,"increasedMuscles":false}

 Sadly the fight has destroyed the bar so he cannot eat anything. Furious, Goku transforms into a golden gorilla!

    {"name":"Golden Oozaru","hairColor":"Blond hair",
    "size":"Giant monkey","hairUp":true,"increasedMuscles":false}

 Then he founds an orange-tree and he eat some oranges. That makes he calm down and he fell asleep, still in golden form

    {"name":"Golden Oozaru","hairColor":"Blond hair",
    "size":"Giant monkey","hairUp":true,"increasedMuscles":false}

 After that we fell asleep and he reverts its form to Full power Super saiyan

    {"name":"Full power super saiyan","hairColor":"Blond hair",
    "size":"Normal size","hairUp":true,"increasedMuscles":false}

 Now he wakes up hungry! Fortunately he found some fishes in the river and he calms down to Super saiyan

    {"name":"Super saiyan","hairColor":"Blond hair",
    "size":"Normal size","hairUp":true,"increasedMuscles":true}

 Mmm, too much light in this form. He goes down to False Super saiyan

    {"name":"False saiyan","hairColor":"Red hair",
    "size":"Normal size","hairUp":true,"increasedMuscles":true}

 Oh, no, night again. Back to the gorilla form!

    {"name":"Oozaru","hairColor":"Black hair",
    "size":"Giant monkey","hairUp":false,"increasedMuscles":false}

 The next morning he awakes normal again.

    {"name":"Normal Goku","hairColor":"Black hair",
    "size":"Normal size","hairUp":false,"increasedMuscles":false}
 
 
# Conclusion
 
 Can you see what happened here? I created a repository of forms that can stack forms so we can retrieve the last form by removing it from the stack. It's the most primitive way of storing history, but it can explain how you can make your own history objects. And that, friends, is one of this things that *everybody* take it for granted.