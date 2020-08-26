---
title: "Python Classes"
excerpt: "Classes provide a means of bundling data and functionality together."
---

### Methods
Methods are functions within a classes


### Self
*self* is a reference to the current instance of the class. It is used to access variables that belong to the class.

### (https://docs.python.org/3/tutorial/classes.html)

> Python classes provide all the standard features of Object Oriented Programming: the class inheritance mechanism allows multiple base classes, a derived class can override any methods of its base class or classes, and a method can call the method of a base class with the same name. Objects can contain arbitrary amounts and kinds of data. As is true for modules, classes partake of the dynamic nature of Python: they are created at runtime, and can be modified further after creation.


### What does __init__() do?

> When a class defines an __init__() method, class instantiation automatically invokes __init__() for the newly-created class instance.

### What about this ->
Here's an example:
> add_node(self, id: str, name: str)->None

That just means the method should return nothing. This is called a *type annotation*.

[This stackoverflow](https://stackoverflow.com/questions/38286718/what-does-def-main-none-do) answer addresses the problem.

### Pass
Classes can't be empty but you can use *pass* to prevent getting an error.


### NotImplemented
*NotImplemented* can be returned that basically just returns nothing. 
