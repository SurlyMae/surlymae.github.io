---
date: "2018-11-15"
title: What is dependency injection? (blog post) 
---

_Dependency injection is a principle of object-oriented programming. But what does it actually mean?_

In the world of object-oriented programming (OOP), applications are designed using, well, objects! Everything in an app is constructed as an object, with defined attributes it has and actions it performs. These objects have to interact with each other to make the app work - for instance, Object A needs to ask Object B to do something, and therefore Object A needs to know the things Object B can do. This needing to know about other objects creates dependencies, which are necessary to a degree, but it's the way you write those dependencies into the objects that matters. You can either basically 'hardcode' the dependencies into the objects, or you can 'inject' them. Let's look at some code.

_I'm using adapted code examples from the [POODR book](https://www.poodr.com/) by [Sandi Metz](https://www.sandimetz.com/) because she is smart and awesome. I learned about dependency injection (DI) from her book and in order to do that I recreated her Ruby examples in C#, so it made sense that I would re-use those examples here. If you don't know about Sandi and want to learn more about object-oriented design, I cannot recommend her enough!_

In this code sample, we have a Gear class (every gear object will be an instance of this Gear class) with chainring, cog, rim, and tire attributes, and two actions it performs - calculating gear inches and ratio. We also have a Wheel class which has rim and tire attributes and one action it performs - calculating diameter. You would be expected to instantiate a gear object by passing in values for its chainring, cog, rim, and tire sizes (see line 49). Then you would call the gear object's GetGearInches method as shown on line 50.

<script src="https://gist.github.com/SurlyMae/995848ee79c86c44bc9ff2aad5c669b7.js"></script>

Where are the dependencies? Check out lines 19-26 in this code sample:

<script src="https://gist.github.com/SurlyMae/f16414c5872b6d8d5d6493667bb7f522.js"></script>

Notice how the Gear class knows a lot about the Wheel class? Consider this a hard-coded dependency!

So, how would we even start to address this issue? Let's begin by dealing with the rim and tire that the Gear class knows about. Gear thinks it needs to know about rim and tire because the GetDiameter method that Gear uses needs a rim and tire. But really, Gear just needs any object which has a GetDiameter method. It takes a bit of work in a statically-typed language like C#, but here are the steps:

First, you would define an interface (lines 51-54 in the code sample below), and in that interface you would name an abstract member - a GetDiameter method. Then, you would make the Wheel class implement the interface you created (this happens on line 34). Wheel already has its own version of the GetDiameter method so it's good to go, but if it didn't, you would need to write a GetDiameter method for the Wheel class since this class is now implementing the IRoundThings interface. Now you can remove the rim and tire attributes from the Gear class and replace them with a SomethingRound attribute of type IRoundThings (line 9), and instead of passing in rim and tire values when instantiating a gear object, you can pass in anything that implements the IRoundThings interface (line 58) - because those things must all have a GetDiameter method, and that GetDiameter method is what's actually needed by the Gear class.

<script src="https://gist.github.com/SurlyMae/784fbef23b595c471c8887af99d196c5.js"></script>

Look at that - we've removed a hard-coded dependency and injected it instead. But wait, we're not done yet! Look at line 22. As Sandi points out in POODR, what if the GetGearInches method was much more complex, and had this dependency buried inside it? And then what if that other object's GetDiameter method changed and, because other parts of the GetGearInches method were so dependent on the GetDiameter method, that you had to change all those parts too? Should we separate the GetDiameter method? (We should.) Now the GetGearInches method is more abstract.

<script src="https://gist.github.com/SurlyMae/f0a73436e1eb143056c8c7c34e117ec8.js"></script>

There is another way we can improve our code. Note how Gear's constructor (lines 11-16 below) is defined with its parameters in a certain order. This means that when a gear object is created elsewhere in the app, the instantiator of the object needs to know the correct order to pass those initializing arguments. That's a dependency, and with C#'s 'Named Arguments' feature, it's simple to get rid of. Line 54 shows the new way of creating a gear object - a way where the instantiator only needs to know the arguments required, not the order in which they must be sent. I think it's also easier to read than the way it was done in all the previous code samples. Yay readability!!

<script src="https://gist.github.com/SurlyMae/feaf7e6d74f47ce8f05c5ec401b0bef2.js"></script>

There are more dependency-related improvements which can be made to this code, but hopefully I've given you a good start on understanding what dependency injection is. Feel free to hit me up on Twitter with your thoughts, and if this sort of stuff interests you, definitely check out Sandi Metz and all of her great work. She is a fantastic teacher.
