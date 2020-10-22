---
date: "2019-03-15"
title: Object-oriented design (notes)
---

_Notes from reading Sandi Metz's [Practical Object-Oriented Design in Ruby](https://www.poodr.com/)_

**Faux-OOD:**
- Hiding parts of the code to make things _appear_ to be smaller   

In a well-designed app, classes that are super complex don't change very much, & code that changes a lot is pretty simple - sweet spot of OOP!

**OOD wants these types of objects:**
- Anthropomorphic
    - OOP is a play where you create living beings and make a world where action happens
- Polymorphic
    - different kinds of objects can respond to same message. Share a common form
- Loosely-coupled
    - objects strive for independence
- Role-playing
    - objects are more players of their roles than instances of their type. Substitutable
- Factory-created
    - factories hide the rules for picking the right player of a role
- Message-sending
    - 'I know what I want, you know how to do it.' They give ignorance

1. Decide roles - what is the common thing they do - inject a smarter thing
2. Isolate the things you want to vary
3. Push conditionals up the stack, back to the first place someone could have had enough info to pick the right object. Factories are where conditionals go.
