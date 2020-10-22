---
date: "2018-11-15"
title: Java beginner notes
---

_Beginner notes from CompSci 101_

**Public:**
- Indicates there are no special restrictions on the use of the method.

**Methods:**
- Every method belongs to some class & is available to objects created from that class. The definition of a method is given in the definition of the class to which it belongs.

**Instance variables:**
- Data fields belonging to each instance of the class (each object). Instance variables are written differently depending on whether they are within the class (`this.instanceVariable` or `instanceVariable`) or outside of the class (as inside a program that uses the class, i.e. `receivingObject.instanceVariable`)

**This:**
- Within a method definition, you can use the keyword `this` as a name for the object receiving the method call

**Local variables:**
- A variable declared within a method. All variables declared in a program's main method are local to main. If you decide to declare a variable within a block, variable is local to the block. So the scope of a local variable extends from its declaration to the end of the block containing the declaration. Formal parameters are local variables.

**Precondition:**
- States the conditions that must be true before the method is invoked

**Postcondition:**
- Describes all the effects produced by a method invocation

**Public vs private:**
- The public modifier, when applied to a class, method, or instance variable, means that any other class can directly use or access the class/method/variable by name.
- public and private are called access modifiers
- When an instance variable is private, its name is not accessible outside of the class definition. But you can still use its name within any method inside the class definition.
- Methods can be private - they cannot be invoked outside the class definition. A private method can still be invoked within the definition of other methods in the same class.

**Getters/Setters:**
- Getter = get method = accessor
- Setter = set method = mutator

**Encapsulation:**
- When done correctly, it neatly divides a class definition into two parts: the interface and the implementation
- A class interface tells programmers all they need to know to use the class. Consists of headings for the public methods & public named constants of the class.
- Implementation consists of all the private elements of the class definition, principally the private instance variables, along with definitions of public and private methods.
- When defining a class while using encapsulation, separate the class interface from the implementation conceptually so the interface is a safe and simplified description of the class.

**Encapsulation how-to:**
- Declare all instance variables as private
- Provide public accessor methods to retrieve data in an object. Also provide public methods for any basic needs a programmer would have for manipulating the data in the class
- Make helper methods private
- Comments, comments, comments

**Interface vs class interface vs API:**
- The API for a class is essentially the same thing as the class interface for the class. You'll often see the term API when reading documentatin for class variables.

**Class type variables:**
- This variable type contains the memory address of the object; this is a reference to the object; class types are often called reference types.

**Params of class type:**
- You can change the actual object values when passing in a class type variable to a method; you cannot replace the object, however

**Constructors:**
- Methods used to create a new object.
- Meant to perform initializing actions.
- It is normal practice to explicitly give values to all instance variables in a constructor
- Constructor can call methods within its class. Make the method called by the constructor private.
- You can call constructors from a constructor. Must be the first action taken within a constructor.

**Static variables & methods:**
- Belong to the class and not the object
- Static variables that aren't constants should normally be private.
- Also known as class variables
- Can allow objects to communicate with each other
- Static methods should be used when method has no relation to any object.
- Static methods cannot reference instance variables. They don't need an object to be instantiated to use them, so they don't have an object's instance variable to refer to. Cannot call a non-static method without first creating an object to reference that method.

**Overloading:**
- Giving two or more methods the same name within the same class; they must each have something different about their parameter lists.
- Java tries to use overloading before it tries to use automatic type conversion
- You cannot overload on basis of return type.

**Inheritance:**
- Allows you to define a very general class & then later define more specialized classes that add some new details to the existing general class definition.
    - Derived class (aka subclass):
        - A class defined by adding instance variables & methods to an existing class. Extends the existing class. Existing class that the derived class is built upon is called the base class or superclass.
        - Private instance variables of the superclass are not directly accessible from derived classes.
        - Does not inherit constructors from the base class, it has its own. Calling a constructor from the base class uses the word `super`. `this` calls a constructor of the same class, while `super` invokes a constructor of the base class.
        - You can assign an object of a class to a variable of any ancestor type, but not the other way around.
    - Is-a:
        - Use inheritance only if an 'is-a' relationship exists between a class and a proposed derived class (i.e. a student is a person, so student can inherit from person)
    - Overriding methods:
        - Derived classes can override methods in the parent class.
        - Method heading, return type, number of parameters has to be the same, but the method body can be different. This method definition is then the one used for objects of the derived class.
    - Prohibit overriding:
        - You can add the `final` modifier to a method heading to keep derived classes from overriding it. You can also mark a class as final, which prohibits using it as a superclass. If a constructor calls a public method, mark the public method as `final`.

**Enumerations:**
- An enumeration lists the values that a variable can have (i.e. `enum GRADE {A, B, C}`)
- Acts as a class type (i.e. `GRADE finalGrade = GRADE.A`)
- Enumeration values behave much like named constants, so use same naming convention (all caps)
- An enumeration is actually a class. You typically define it within another class, but outside method definitions.
- The compiler creates a class when it encounters an enumeration
    - example: `enum SUIT {CLUBS, DIAMONDS}`: Compiler creates a class `SUIT`. The enumerated values are names of public static objects whose type is `SUIT`
- You can define private instance variables, public methods, constructors for any enum.
- Enums can be private or public.

**Packages:**
- A named collection of related classes that can serve as a library of classes for use in any program. With packages, you do not need to place all those classes in the same directory as your program.
- A package is simply a collection of classes that have been grouped together in a folder. The name of the folder is the name of the package. The classes in the package are each placed in a separate file, and the file name begins with the name of the class.

**.class:**
- `.class` is used when there isn't an instance of the class available
- "Refers to the class itself, rather than an instance of the class. A compiled `.java` file is a `.class` file. They do this because of Type Erasure. Something that Java does that .NET doesn't. If they did `getForObject<T>` that `T` is only known at compile time. This is so that it exists at compile time as well."