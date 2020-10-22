---
date: "2018-12-15"
title: What are automatic properties in C#? (blog post)
---

_Ever seen C# code like this and wondered what's going on?_   
`public int Name { get; set; }`  
_I've got you covered._

**_This post still in draft mode_**

One of the pillars of object-oriented programming (OOP) is encapsulation. We can think of encapsulation in two ways: First, when we're designing classes, how do we 'hide' the details of the code from those who don't need to see it? And second, how do we protect the data belonging to each object?

So why are we starting off by discussing encapsulation when this post is supposed to be about automatic properties? Because automatic properties provide simpler encapsulation!

Let's look at a traditional way of implementing encapsulation:

<script src="https://gist.github.com/SurlyMae/77770f5492632387a089a3538d579654.js"></script>

This is a student class. (Remember, a class is just a blueprint for an object's design. So when you create a student object, this blueprint is used to say, "Hey, that student object needs these things!") Each student has an id, a first and last name, and a preferred first name. We declare these as private so that other objects can't access them directly. Then we provide public methods for getting and setting those private values - now other objects can access the information, but in a way that the student object controls.

Now, let's do our first rewrite:

<script src="https://gist.github.com/SurlyMae/891916083a117d1ddd8bdf2cff672db5.js"></script>

Moving top down, let's look at the things we've changed. The constructor is different - instead of `this.id = id`, we now see `StudentId = id`. Um, where in the heck did `StudentId` come from? Take a look just below the constructor, where you'll see `public int StudentId { ... }`. At first glance this looks like a public method called `StudentId` and returning an `int`, but notice that there are no parentheses following `StudentId`. In C#, this is a property and it represents a private field - in this case, it is representing the `private int id` field. It also provides a shorthand way of writing getters and setters. Notice that lines 18-26 in the traditional example have been replaced by lines 19-25 in this first rewrite-. So, in the constructor, when we see `StudentId = id`, what's happening is this:

1. When a student object is initialized, the constructor is called.
2. The constructor grabs the id parameter being passed in (let's say it's '1234') and tries to set `StudentId` to `1234` (line 13).
3. The constructor looks for `StudentId`, and finds it on line 19. The setter on line 23 is hit, and `id` is set to `1234`. Now, _this_ `id` is the `private int id` from line 6, _not_ the id parameter passed in to the constructor.
4. So now `private int id` is set to `1234`. BOOM.

And then the constructor just keeps going, and repeats the process for the other property names.

With this rewrite, we've gotten rid of some lines of code, which is usually beneficial. Can we get rid of even more code by simplifying this class further? Hmm, let's see:

<script src="https://gist.github.com/SurlyMae/8553dd325ee5b4e709e148ec46d427ae.js"></script>

The constructor looks the same, but we've now replaced _most_ of the private data fields (lines 5-11) with properties. Lines 19-25 from the first rewrite have been replaced by line 8 in this rewrite, lines 43-47 have been replaced by line 9, and lines 49-53 have been replaced by line 10. Lines 27-41 have been replaced by lines 11-25, and that code is mostly the same as before, except for one thing - can you spot the difference?

A gotcha!:

<script src="https://gist.github.com/SurlyMae/f1adb62a52a4f1b4c587409b444694dc.js"></script>
