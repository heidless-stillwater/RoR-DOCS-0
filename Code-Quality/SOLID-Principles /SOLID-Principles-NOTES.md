
## [SOLID Principles for Programming and Software Design](https://www.freecodecamp.org/news/solid-principles-for-programming-and-software-design/)

SOLID is a mnemonic acronym that stands for the five design principles of Object-Oriented class design. These principles are:

### S - [Single-responsibility Principle (SRP)](https://en.wikipedia.org/wiki/Single-responsibility_principle) 

The Single-responsibility Principle, or SRP, states that a class should only have one reason to change. This means that **a class should have only one job and do one thing**.

### O - [Open-closed Principle (OCP)](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)

The open-closed principle can be confusing because it's a two-direction principle. 

There are two primary attributes in the OCP:

- A module will be said to be open if it is still available for extension. For example, it should be possible to add fields to the data structures it contains, or new elements to the set of functions it performs.

- A module will be said to be closed if [it] is available for use by other modules. This assumes that the module has been given a well-defined, stable description (the interface in the sense of information hiding).

It is open for extension — This means you can extend what the module can do.

It is closed for modification — This means you cannot change the source code, even though you can extend the behavior of a module or entity.

### L - Liskov Substitution Principle (LSP)
In 1987, the Liskov Substitution Principle (LSP) was introduced by Barbara Liskov in her conference keynote “Data abstraction”. A few years later, she defined the principle like this:

“Let Φ(x) be a property provable about objects x of type T. Then Φ(y) should be true for objects y of type S where S is a subtype of T”.

"**an object (such as a class) may be replaced by a sub-object (such as a class that extends the first class) without breaking the program.**"

The principle defines that in an inheritance, objects of a superclass (or parent class) should be substitutable with objects of its subclasses (or child class) without breaking the application or causing any error.

In very plain terms, **you want the objects of your subclasses to behave the same way as the objects of your superclass**.

### I - [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle#:~:text=In%20the%20field%20of%20software,are%20of%20interest%20to%20them.) (ISP)

The Interface Segregation Principle (ISP) states that “**a client should never be forced to implement an interface that it doesn’t use**, or clients shouldn’t be forced to depend on methods they do not use”.

A typical interface will contain methods and properties. When you implement this interface into any class, then the class needs to define all its methods. 

```
interface ShapeInterface {
    calculateArea();
    calculateVolume();
}

```

When any class implements this interface, all the methods must be defined even if you won't use them or if they don’t apply to that class.

To fix this, you would need to segregate the interface.

```
interface ShapeInterface {
    calculateArea();
}

interface ThreeDimensionalShapeInterface {
    calculateArea();
    calculateVolume();
}
```

You can now implement the specific interface that works with each class.


### D - [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) (DIP)

This principle is targeted towards loosely coupling software modules so that high-level modules (which provide complex logic) are easily reusable and unaffected by changes in low-level modules (which provide utility features).

According to [Wikipedia](https://en.wikipedia.org/wiki/Dependency_inversion_principle), this principle states that:

- High-level modules should not import anything from low-level modules. Both should depend on abstractions (for example, interfaces).

- Abstractions should be independent of details. Details (concrete implementations) should depend on abstractions.

In plain terms, this principle states that **your classes should depend upon interfaces or abstract classes instead of concrete classes and functions***. This makes your classes open to extension, following the open-closed principle.

You will also notice that this follows the Liskov Substitution Principle because you can replace it with other implementations of the same interface without breaking your application.


