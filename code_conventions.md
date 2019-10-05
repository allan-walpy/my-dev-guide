# code conventions

> this code conventions are a modification of [dennisdoomen/CSharpGuidelines](https://github.com/dennisdoomen/CSharpGuidelines/releases/tag/5.4.0);

## basic principles

### least surprise principle

> also Astonishment;

choose a solution everyone can understand and keeps on right track;

### keep it simple stupid

> also KISS;

the simplest solution is more than sufficient;

### you ain't gonna need it

> also YAGNI;

create a solution for the problem at hand, not for the future one;

### don't repeat yourself

> also DRY;

avoid duplication within a component, a source control repository or a bounded
context

### rule of three

> see also [abstraction: the rule of three](https://lostechies.com/derickbailey/2012/10/31/abstraction-the-rule-of-three/);

*if you need something* **once**, **build** it;

*if you need something* **twice**, **pay attention** to it;

*if you need something* **a third time**, **abstract** it;

### principles of object oriented programming

> see also [object-oriented programming fundamental principles](https://github.com/TelerikAcademy/Object-Oriented-Programming/tree/master/Topics/04.%20OOP-Principles-Part-1);

**encapsulation**: bundling data with the methods operating on that data;

**abstraction**: keep common features/attributes in various concrete objects;

**inheritance**: basing class upon another, retaining similar implementation;

**polymorphism**: providing a single interface to different types entities;

## class design

### single responsibility principle

`AV1000 a class or interface should have a single purpose`

**must**: a class or interface should have a single purpose;

in general, a class either represents a primitive type like an email or ISBN
number, an abstraction of some business concept, a plain data structure, or is
responsible for orchestrating the interaction between other classes; it is
never a combination of those;

> a class with the word `and` in it is an obvious violation of this rule;
>
> use design patterns to communicate the intent of a class; if you can't assign
> a single design pattern to a class,may be it is doing more than one thing;
>
> if you create a class representing a primitive type you can greatly simplify
> its use by making it immutable;

### useful constructor

`AV1001 only create a constructor that returns a useful object`

**suggestion**: only create a constructor that returns a useful object;

there should be no need to set additional properties before the object can be used
for whatever purpose it was designed;

> if constructor needs more than 3 parameters,
> class might have too much responsibility;

### interface segregation principle

`AV1003 an interface should be small and focused`

**recommendation**: an interface should be small and focused;

>interfaces should have a name that clearly explains purpose/role;
>
> do not combine many vaguely related members on the same interface just
> because they were all on the same class;
>
> separate the members based on the responsibility of those, so that callers
> only need to call/implement the interface related to a particular task;

### favor interface to base class

`AV1004 use an interface rather than a base class to support multiple implementations`

**suggestion**: use an interface rather than a base class to support multiply implementations;

> if there is need to expose an extension point from class,
>expose it as an interface rather than as a base class;
>
> you may implement an abstract default implementation of
> interface that can serve as a starting point

### use interfaces to decouple classes from each other

`AV1005 use an interface to decouple classes from each other`

**recommendation**: interfaces are a very effective mechanism for decoupling
classes from each other; they are:

- preventing bidirectional associations;

- simplify replacing one implementation with another;

- allow the replacement of an expensive external service or resource with
  a temporary stub for use in a non-production environment;

- allow the replacement of the actual implementation with a dummy/fake one;

- with using a dependency injection framework allow to centralize the choice
  of which class is used whenever a specific interface is requested;

### avoid static classes

`AV1008 avoid static classes`

**suggestion**: static classes very often lead to badly designed code;
they are also very difficult, if not impossible, to test in isolation;

> exception can be the exception of extension method containers;
>
> if you really need that static class, mark it as static so that the compiler
> can prevent instance members and instantiating your class; this relieves you
> of creating an explicit private constructor;

### don't suppress compiler warnings using the new keyword

`AV1010 don't suppress compiler warnings using the new keyword`

**must**: it should not make a difference whether you call method through
a reference to the base class or through the derived class;

> [compiler warning CS0114](https://docs.microsoft.com/en-us/dotnet/csharp/misc/cs0114)
> is issued when breaking
> [polymorphism](#principles-of-object-oriented-programming)
> one of the most essential object-orientation principles;

### liskov substitution principle

`AV1011 it should be possible to treat a derived object as if it were a base
class object`

**recommendation**: you should be able to use a reference to an object of
a derived class wherever a reference to its base class object is used without
knowing the specific derived class;

> violations of this rule:
>
> throwing a `NotImplementedException` when overriding some of the base-class
> methods;
>
> not honoring the behavior expected by the base class;

### don't refer to derived classes from the base class

`AV1013 don't refer to derived classes from the base class`

**must**: having dependencies from a base class to its sub-classes goes
against proper object-oriented design and might prevent other developers from
adding new derived classes;

### avoid exposing the other objects an object depends on

`AV1014 avoid exposing the other objects an object depends on`

> see also [law of demeter](https://en.wikipedia.org/wiki/Law_of_Demeter)

**recommendation**: an object should not expose any other classes it depends
on because callers may misuse that exposed property or method to access the
object behind it;

doing so, you allow calling code to become coupled to the class you are using,
and thereby limiting the chance that you can easily replace it in a future;

- each unit should have only limited knowledge about other units: only units
  *closely* related to the current unit;

- each unit should only talk to its friends;

- only talk to your immediate friends;

> using a class that is designed using the [fluent interface pattern](https://en.wikipedia.org/wiki/Fluent_interface)
> seems to violate this rule, but it is simply returning itself so that method
> chaining is allowed;
>
> [dependency injection frameworks](https://en.wikipedia.org/wiki/Dependency_injection#Dependency_injection_frameworks)
> often require you to expose a dependency as a public property; as long as
> this property is not used for anything other than dependency injection I
> would not consider it a violation;

### avoid bidirectional dependencies

`AV1020 avoid bidirectional dependencies`

**must**: two classes know about each other's public members or rely on
each other's internal behavior: refactoring or replacing one of those classes
requires changes on both parties and may involve a lot of unexpected work;

> obvious way of breaking that dependency is to introduce an interface for one
> of the classes and using Dependency Injection;
>
> domain models such as defined in [domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design)
> tend to occasionally involve bidirectional associations that model real-life
> associations; in those cases, make sure they are really necessary, and if
> they are, keep them in;

### classes should have state and behavior

`AV1025 classes should have state and behavior`

**must**: if you find a lot of data-only classes in your code base, you probably
also have a few *static* classes with a lot of behavior;

> use [the principles of object-orientation](#principles-of-object-oriented-programming)
> and move the logic close to the data it applies to;
>
> the only exceptions to this rule are classes that are used to transfer data
> over a communication channel, also called [data transfer objects](https://martinfowler.com/eaaCatalog/dataTransferObject.html)
> or a class that wraps several parameters of a method;

### classes should protect the consistency of their internal state

`AV1026 classes should protect the consistency of their internal state`

**must**: validate incoming arguments from public members and protect invariants
on internal state;

```csharp
public void SetAge(int years)
{
    // validate;
    AssertValueIsInRange(years, 0, 200, nameof(years));

    this.age = years;
}

public void Render()
{
    // protect;
    AssertNotDisposed();

    // ...
}
```
