# code conventions

> this code conventions are a modification of [dennisdoomen/CSharpGuidelines](https://github.com/dennisdoomen/CSharpGuidelines/releases/tag/5.4.0)
> with some addition by [allan_walpy](https://github.com/allan-walpy);

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

`AV1000 A class or interface should have a single purpose`

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

`AV1001 Only create a constructor that returns a useful object`

**suggestion**: only create a constructor that returns a useful object;

there should be no need to set additional properties before the object can be used
for whatever purpose it was designed;

> if constructor needs more than 3 parameters,
> class might have too much responsibility;

### interface segregation principle

`AV1003 An interface should be small and focused`

**recommendation**: an interface should be small and focused;

>interfaces should have a name that clearly explains purpose/role;
>
> do not combine many vaguely related members on the same interface just
> because they were all on the same class;
>
> separate the members based on the responsibility of those, so that callers
> only need to call/implement the interface related to a particular task;

### favor interface to base class

`AV1004 Use an interface rather than a base class to support multiple implementations`

**suggestion**: use an interface rather than a base class to support multiply implementations;

> if there is need to expose an extension point from class,
>expose it as an interface rather than as a base class;
>
> you may implement an abstract default implementation of
> interface that can serve as a starting point

### use interfaces to decouple classes from each other

`AV1005 Use an interface to decouple classes from each other`

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

`AV1008 Avoid static classes`

**suggestion**: static classes very often lead to badly designed code;
they are also very difficult, if not impossible, to test in isolation;

> exception can be the exception of extension method containers;
>
> if you really need that static class, mark it as static so that the compiler
> can prevent instance members and instantiating your class; this relieves you
> of creating an explicit private constructor;

### don't suppress compiler warnings using the `new` keyword

`AV1010 Don't suppress compiler warnings using the new keyword`

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

`AV1013 Don't refer to derived classes from the base class`

**must**: having dependencies from a base class to its sub-classes goes
against proper object-oriented design and might prevent other developers from
adding new derived classes;

### avoid exposing the other objects an object depends on

`AV1014 Avoid exposing the other objects an object depends on`

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

`AV1020 Avoid bidirectional dependencies`

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

`AV1025 Classes should have state and behavior`

**must**: if you find a lot of data-only classes in your code base, you probably
also have a few *static* classes with a lot of behavior;

> use [the principles of object-orientation](#principles-of-object-oriented-programming)
> and move the logic close to the data it applies to;
>
> the only exceptions to this rule are classes that are used to transfer data
> over a communication channel, also called [data transfer objects](https://martinfowler.com/eaaCatalog/dataTransferObject.html)
> or a class that wraps several parameters of a method;

### classes should protect the consistency of their internal state

`AV1026 Classes should protect the consistency of their internal state`

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

## member design

### allow properties to be set in any order

`AV1100 Allow properties to be set in any order`

**must**: properties should be stateless with respect to other properties;

there should not be a difference between first setting property `DataSource` and
then `DataMember` or vice-versa;

### use a method instead of a property

`AV1105 Use a method instead of a property`

**suggestion**: use a method instead of a property;

- if the work is more expensive than setting a field value;

- if it represents a conversion such as the `Object.ToString` method;

- if it returns a different result each time it is called, even if the arguments
  didn't change;

  > example: the `NewGuid` method returns a different value each time it is called;

- if the operation causes a side effect such as changing some internal state not
  directly related to the property

  > this violates the [command query separation principle](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation);

> exception: populating an internal cache or implementing lazy-loading;

### don't use mutually exclusive properties

`AV1110 Don't use mutually exclusive properties`

**must**: having properties that cannot be used at the same time typically
signals a type that represents two conflicting concepts; even though those
concepts may share some of their behavior and states, they obviously have
different rules that do not cooperate;

> this violation is often seen in domain models and introduces all kinds of
> conditional logic related to those conflicting rules, causing a ripple effect
> that significantly increases the maintenance burden;

### a property, method or local function should do only one thing

`AV1115 A property, method or local function should do only one thing`

**must**: a method body should have a single responsibility;

### don't expose stateful objects through static members

`AV1125 Don't expose stateful objects through static members`

> a stateful object is an object that contains many properties and lots of
> behavior behind it;

**recommended**: if you expose such an object through a static property or method
of some other object, it will be very difficult to refactor or unit test a class
that relies on such a stateful object;

> example: the `HttpContext.Current` property, part of `ASP.NET`; many see the
> `HttpContext` class as a source of a lot of ugly code; in fact, the testing
> [guideline isolate the ugly stuff](http://codebetter.com/jeremymiller/2005/10/21/haacked-on-tdd-and-jeremys-first-rule-of-tdd/)
> often refers to this class;

### return a builtin collection interface instead of a concrete collection class

`AV1130 Return an IEnumerable<T> or ICollection<T> instead of a concrete
collection class`

**recommendation**: you generally don't want callers to be able to change an
internal collection, so don't return arrays, lists or other collection classes
directly; instead, return an `IEnumerable<T>`, `ICollection<T>` or `IList<T>`;

> you can also use `IReadOnlyCollection<T>`, `IReadOnlyList<T>` or
> `IReadOnlyDictionary<TKey, TValue>`;
>
> exception: immutable collections such as `ImmutableArray<T>`,
> `ImmutableList<T>` and `ImmutableDictionary<TKey, TValue>` prevent
> modifications from the outside and are thus allowed

### strings or collections should never be `null`

`AV1135 Properties, arguments and return values representing strings or
collections should never be null`

**must**: returning `null` can be unexpected by the caller; always return an
empty collection or an empty string instead of a `null` reference.

> when your member return `Task` or `Task<T>`, return `Task.CompletedTask` or
> `Task.FromResult()`; this also prevents cluttering your code base with
> additional checks for null, or even worse, `string.IsNullOrEmpty()`;

### define parameters as specific as possible

`AV1137 Define parameters as specific as possible`

> *don't ship the truck if you only need a package*

**recommendation**: if your method or local function needs a specific piece of
data, define parameters as specific as that and don't take a container object
instead;

> for instance, consider a method that needs a connection string that is exposed
> through a central `IConfiguration` interface; rather than taking a dependency
> on the entire configuration, just define a parameter for the connection string;
> this not only prevents unnecessary coupling, it also improves maintainability
> in the long run;

### consider using domain-specific value types rather than primitives

`AV1140 Consider using domain-specific value types rather than primitives`

**suggestions**: instead of using strings, integers and decimals for representing
domain-specific types such as an ISBN number, an email address or amount of money,
consider creating dedicated value objects that wrap both the data and the
validation rules that apply to it; by doing this, you prevent ending up having
multiple implementations of the same business rules, which both improves
maintainability and prevents bugs;

## exception design

### throw exceptions rather than returning some kind of status value

`AV1200 Throw exceptions rather than returning some kind of status value`

**recommendation**: a code base that uses return values to report success or
failure tends to have nested if-statements sprinkled all over the code; quite
often, a caller forgets to check the return value anyway; structured exception
handling has been introduced to allow you to throw exceptions and catch or
replace them at a higher layer; in most systems it is quite common to throw
exceptions whenever an unexpected situation occurs;

### provide a rich and meaningful exception message text

`AV1202 Provide a rich and meaningful exception message text`

**recommended**: the message should explain the cause of the exception,
and clearly describe what needs to be done to avoid the exception;

### throw the most specific exception that is appropriate

`AV1205 Throw the most specific exception that is appropriate`

**suggestion**: throw the most specific exception that is appropriate;

> example: if a method receives a `null` argument, it should throw
> `ArgumentNullException` instead of its base type `ArgumentException`;

### don't swallow errors by catching generic exceptions

`AV1210 Don't swallow errors by catching generic exceptions`

**must**: avoid swallowing errors by catching non-specific exceptions, such as
`Exception`, `SystemException` and so on, in application code; only in top-level
code, such as a last-chance exception handler, you should catch a non-specific
exception for logging purposes and a graceful shutdown of the application;

### properly handle exceptions in asynchronous code

`AV1215 Properly handle exceptions in asynchronous code`

**recommended**: when throwing or handling exceptions in code that uses
`async`/`await` or a `Task` remember the following two rules;

- exceptions that occur within an `async`/`await` block and inside a `Task`'s
  action are propagated to the awaiter;

- exceptions that occur in the code preceding the asynchronous block are
  propagated to the caller;

## event design

### always check an event handler delegate for `null`

`AV1220 Always check an event handler delegate for null`

**must**: an event that has no subscribers is `null`; so before invoking, always
make sure that the delegate list represented by the event variable is not `null`;
invoke using the `null` conditional operator, because it additionally prevents
conflicting changes to the delegate list from concurrent threads;

```csharp
event EventHandler<NotifyEventArgs> Notify;

protected virtual void OnNotify(NotifyEventArgs args)
{
    Notify?.Invoke(this, args);
}
```

### use a protected virtual method to raise each event

`AV1225 Use a protected virtual method to raise each event`

**recommended**: complying with this guideline allows derived classes to handle
a base class event by overriding the protected method; the name of the protected
virtual method should be the same as the event name prefixed with `On`;

> example: the protected virtual method for an event named `TimeChanged` is named
> `OnTimeChanged`;
>
> derived classes that override the protected virtual method are not required to
> call the base class implementation; the base class must continue to work
> correctly even if its implementation is not called;

### consider providing property-changed events

`AV1230 Consider providing property-changed events`

**suggestion**: consider providing events that are raised when certain properties
are changed; such an event should be named `PropertyChanged`;

> if your class has many properties that require corresponding events, consider
> implementing the `INotifyPropertyChanged` interface instead. It is often used
> in the [presentation model](http://martinfowler.com/eaaDev/PresentationModel.html)
> and [model-view-viewModel](https://msdn.microsoft.com/en-us/magazine/dd419663.aspx)
> patterns;

### don't pass `null` as the sender argument when raising an event

`AV1235 Don't pass null as the sender argument when raising an event`

**must**: often an event handler is used to handle similar events from multiple
senders; the sender argument is then used to get to the source of the event;
always pass a reference to the source (typically `this`) when raising the event;
furthermore don't pass `null` as the event data parameter when raising an event;
if there is no event data, pass `EventArgs.Empty` instead of `null`;

> exception: on static events, the sender argument should be `null`;

## miscellaneous design

### use generic constraints if applicable

`AV1240 Use generic constraints if applicable`

**recommendation**: instead of casting to and from the object type in generic
types or methods, use `where` constraints or the `as` operator to specify the
exact characteristics of the generic parameter;

```csharp
class SomeClass
{}

// invalid;
class WrongMyClass
{
    void SomeMethod(T t)
    {
        object temp = t;
        SomeClass obj = (SomeClass) temp;
    }
}

// valid;
class RightMyClass
    where T : SomeClass
{
    void SomeMethod(T t)
    {
        SomeClass obj = t;
    }
}
```

### evaluate the result of a linq expression before returning it

`AV1250 Evaluate the result of a LINQ expression before returning it`

```csharp
public IEnumerable<GoldMember> GetGoldMemberCustomers()
{
    const decimal GoldMemberThresholdInEuro = 1_000_000;

    var query =
        from customer in db.Customers
        where customer.Balance > GoldMemberThresholdInEuro
        select new GoldMember(customer.Name, customer.Balance);

    return query;
}
```

**must**: since linq queries use deferred execution, returning `query` will
actually return the expression tree representing the above query; each time the
caller evaluates this result using a `foreach` loop or similar, the entire query
is re-executed resulting in new instances of `GoldMember` every time;
consequently, you cannot use the `==` operator to compare multiple `GoldMember`
instances; instead, always explicitly evaluate the result of a linq query using
`ToList()`, `ToArray()` or similar methods;

### do not use this and base prefixes unless it is required

`AV1251 Do not use this and base prefixes unless it is required`

**must**: in a class hierarchy, it is not necessary to know at which level a
member is declared to use it; refactoring derived classes is harder if that level
is fixed in the code;

## maintainability

### methods should not exceed 7 statements

`AV1500 Methods should not exceed 7 statements`

**must**: a method that requires more than 7 statements is simply doing too much
or has too many responsibilities; it also requires the human mind to analyze the
exact statements to understand what the code is doing; break it down into
multiple small and focused methods with self-explaining names, but make sure the
high-level algorithm is still clear;

### make all members private and types internal sealed by default

`AV1501 Make all members private and types internal sealed by default`

**must**: to make a more conscious decision on which members to make available to
other classes, first restrict the scope as much as possible; then carefully
decide what to expose as a public member or type;

### avoid conditions with double negatives

`AV1502 Avoid conditions with double negatives`

```csharp
bool hasOrders = !customer.HasNoOrders;
```

**recommendation**: although a property like `customer.HasNoOrders` makes sense,
avoid using it in a negative condition; double negatives are more difficult to
grasp than simple expressions;

### name assemblies after their contained namespace

`AV1505 Name assemblies after their contained namespace`

**suggestion**: all dlls should be named according to the pattern
`Product.Component.dll` where `Product` refers to your product's name and
`Component` contains one or more dot-separated clauses

```text
Vulmy.Client.Web.dll
```

> example: consider a group of classes organized under the namespace
> `Vulmy.Server.Web.Binding` exposed by a certain assembly; according to this
> guideline, that assembly should be called `Vulmy.Server.Web.Binding.dll`;
>
> exception: if you decide to combine classes from multiple unrelated namespaces
> into one assembly, consider suffixing the assembly name with `Core`, but do not
> use that suffix in the namespaces; for instance: `Vulmy.Server.Web.Core.dll`;

### name a source file to the type it contains

`AV1506 Name a source file to the type it contains`

**suggestion**: use `PascalCasing` to name the file and don't use underscores;
don't include (the number of) generic type parameters in the file name;

### limit the contents of a source code file to one type

`AV1507 Limit the contents of a source code file to one type`

**suggestion**: limit the contents of a source code file to one type;

> exceptions:
>
> nested types should be part of the same file;
>
> types that only differ by their number of generic type parameters should be
> part of the same file;

### name a source file to the logical function of the partial type

`AV1508 Name a source file to the logical function of the partial type`

**suggestion**: when using partial types and allocating a part per file, name
each file after the logical part that part plays;

```csharp

// In MyClass.cs
public partial class MyClass
{...}

// In MyClass.Designer.cs
public partial class MyClass
{...}

```

### use `using` statements instead of fully qualified type names

`AV1510 Use using statements instead of fully qualified type names`

**suggestion**: limit usage of fully qualified type names to prevent name clashing;

```csharp

// invalid;
var list = new System.Collections.Generic.List<string>();

// valid;
using System.Collections.Generic;

var list = new List<string>();
```

> if you do need to prevent name clashing, use a using directive to assign an
> alias;
>
> ```csharp
> using Label = System.Web.UI.WebControls.Label;
> ```

### don't use magic numbers

`AV1515 Don't use "magic" numbers`

> exception: strings intended for logging or tracing;

**must**: don't use literal values, either numeric or strings, in your code, other
than to define symbolic constants;

```csharp
// valid;
public class Whatever
{
    public static readonly Color PapayaWhip = new Color(0xFFEFD5);
    public const int MaxNumberOfWheels = 18;
    public const byte ReadCreateOverwriteMask = 0b0010_1100;
}
```

> literals are allowed when their meaning is clear from the context, and not
> subject to future changes;
>
> ```csharp
> mean = (a + b) / 2;
> WaitMilliseconds(waitTimeInSeconds * 1000);
> ```
>
> if the value of one constant depends on the value of another, attempt to make
> this explicit in the code;
>
> ```csharp
> public class SomeSpecialContainer
> {
>     public const int MaxItems = 32;
>     public const int HighWaterMark = 3 * MaxItems / 4; // at 75%
> }
> ```
>
> an enumeration can often be used for certain types of symbolic constants;

### only use `var` when the type is very obvious

`AV1520 Only use var when the type is very obvious`

> see also [uses and misuses of implicit typing](https://blogs.msdn.microsoft.com/ericlippert/2011/04/20/uses-and-misuses-of-implicit-typing/);

**must**: only use var as the result of a linq query, or if the type is very
obvious from the same statement and using it would improve readability;

```csharp
// invalid;
var item = 3;
var myFoo = MyFactoryMethod.Create("arg");

// valid;
var query = from order in orders
    where order.Items > 10 and order.TotalValue > 1000;
var repository = new RepositoryFactory.Get();
var list = new ReadOnlyCollection();
```

### declare and initialize variables as late as possible

`AV1521 Declare and initialize variables as late as possible`

**recommendation**: avoid the c/visual basic/pascal styles where all variables
have to be defined at the beginning of a block, but rather define and initialize
each variable at the point where it is needed;

### assign each variable in a separate statement

`AV1522 Assign each variable in a separate statement`

**must**: assign each variable in a separate statement;

```csharp
// invalid;
var result = someField = GetSomeMethod();
```

> exception: multiple assignments per statement are allowed by using out variables,
> is-patterns or deconstruction into tuples
>
> ```csharp
> bool success = int.TryParse(text, out int result);
>
> if ((items[0] is string text) || (items[1] is Action action))
> {
>     // ...
> }
>
> (string name, string value) = SplitNameValuePair(text);
> ```

### favor object and collection initializers over separate statements

`AV1523 Favor object and collection initializers over separate statements`

> see also [object and collection initializers](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers);

**recommendation**: favor object/collection initializers over separate statements;

```csharp
// invalid;
var startInfo = new ProcessStartInfo("myApp.exe");
startInfo.StandardOutput = Console.Output;
startInfo.UseShellExecute = true;

// valid;
var startInfo = new ProcessStartInfo("myApp.exe")
{
    StandardOutput = Console.Output,
    UseShellExecute = true
};

// invalid;
var countries = new List();
countries.Add("Russia");
countries.Add("Netherlands");

// valid;
var countries = new List { "Russia", "Netherlands" };

// invalid;
var countryLookupTable = new Dictionary<string, string>();
countryLookupTable.Add("RU", "Russia");
countryLookupTable.Add("NL", "Netherlands");

// valid;
var countryLookupTable = new Dictionary<string, string>
{
    ["RU"] = "Russia",
    ["NL"] = "Netherlands"
};
```

### don't make explicit comparisons to true or false

`AV1525 Don't make explicit comparisons to true or false`

**must**: it is bad style to compare a bool-type expression to `true` or `false`;

```csharp
// invalid;
while (condition == false) { }
while (condition != true) { }

// valid;
while (condition) { }
```

### don't change a loop variable inside a `for` loop

`AV1530 Don't change a loop variable inside a for loop`

**recommendation**: updating the loop variable within the loop body is generally
considered confusing, even more so if the loop variable is modified in more than
one place;

```csharp
// invalid;
for (int index = 0; index < 10; ++index)
{
    if (someCondition)
    {
        index = 11;
    }
}

// valid;
for (int index = 0; index < 10; ++index)
{
    if (someCondition)
    {
        break;
    }
}
```

### avoid nested loops

`AV1532 Avoid nested loops`

**recommendation**: a method that nests loops is more difficult to understand than
one with only a single loop. In fact, in most cases nested loops can be replaced
with a much simpler linq query that uses the `from` keyword twice or more to join
the data;

### add a block after `if`, `else`, `do`, `while`, `for`, `foreach`, `case`

`AV1535 Always add a block after the keywords if, else, do, while, for, foreach
and case`

**recommendation**: always add a block after the keywords `if`, `else`, `do`,
`while`, `for`, `foreach`, `case`;

> this also avoids possible confusion in statements of the form;
>
> ```csharp
> // invalid;
> if (isActive) if (isVisible) Foo(); else Bar();
>
> // valid;
> if (isActive)
> {
>    if (isVisible)
>    {
>        Foo();
>    }
>    else
>    {
>        Bar();
>    }
>}
> ```

### always add a default block after the last case in a `switch` statement

`AV1536 always add a default block after the last case in a switch statement`

**must**: add a descriptive comment if the default block is supposed to be empty;
moreover, if that block is not supposed to be reached throw an
`InvalidOperationException` to detect future changes that may fall through the
existing cases; this ensures better code, because all paths the code can travel
have been thought about;

```csharp
void Foo(string answer)
{
    switch (answer)
    {
        case "no":
        {
            Console.WriteLine("You answered with No");
            break;
        }

        case "yes":
        {
            Console.WriteLine("You answered with Yes");
            break;
        }

        default:
        {
            throw new InvalidOperationException($"Unexpected answer `{answer}`");
        }
    }
}
```

### finish every if-else-if statement with an `else` clause

`AV1537 Finish every if-else-if statement with an else clause`

**recommendation**: finish every if-else-if statement with an else clause;

```csharp
void Foo(string answer)
{
    if (answer == "no")
    {
        Console.WriteLine("You answered with No");
    }
    else if (answer == "yes")
    {
        Console.WriteLine("You answered with Yes");
    }
    else
    {
        throw new InvalidOperationException($"incorrect answer `{answer}`");
    }
}
```

### be reluctant with multiple `return` statements

`AV1540 Be reluctant with multiple return statements`

**recommendation**: one entry, one exit is a sound principle and keeps control
flow readable; however, if the method body is very small and complies with
[methods should not exceed 7 statements](#methods-should-not-exceed-7-statements)
then multiple return statements may actually improve readability over some central
boolean flag that is updated at various points;

### don't use an if-else construct instead of a simple conditional assignment

`AV1545 Don't use an if-else construct instead of a simple (conditional)
assignment`

**recommendation**: express your intentions directly;

```csharp

// invalid;
bool isPositive;

if (value > 0)
{
    isPositive = true;
}
else
{
    isPositive = false;
}

// valid;
bool isPositive = (value > 0);

// invalid;
string classification;

if (value > 0)
{
    classification = "positive";
}
else
{
    classification = "negative";
}

return classification;

// valid;
return (value > 0) ? "positive" : "negative";

// invalid;
int result;

if (offset == null)
{
    result = -1;
}
else
{
    result = offset.Value;
}

return result;

// valid;
return offset ?? -1;

// invalid;
if (employee.Manager != null)
{
    return employee.Manager.Name;
}
else
{
    return null;
}

// valid;
return employee.Manager?.Name;
```

### encapsulate complex expressions in a property, method or local function

`AV1547 Encapsulate complex expressions in a property, method or local function`

**must**: encapsulate complex expressions in a property, method or local function;

```csharp
if (member.HidesBaseClassMember && (member.NodeType != NodeType.InstanceInitializer))
{
    // do something
}
```

in order to understand what this expression is about, you need to analyze its
exact details and all of its possible outcomes; obviously, you can add an
explanatory comment on top of it, but it is much better to replace this complex
expression with a clearly named method;

```csharp
if (NonConstructorMemberUsesNewKeyword(member))
{
    // do something;
}

private bool NonConstructorMemberUsesNewKeyword(Member member)
{
    return member.HidesBaseClassMember
        && (member.NodeType != NodeType.InstanceInitializer)
}
```

you still need to understand the expression if you are modifying it,
but the calling code is now much easier to grasp;

### call the more overloaded method from other overloads

`AV1551 Call the more overloaded method from other overloads`

**recommendation**: this guideline only applies to overloads that are intended to
provide optional arguments;

```csharp
// valid;
public class MyString
{
    private string someText;

    public int IndexOf(string phrase)
    {
        return IndexOf(phrase, 0);
    }

    public int IndexOf(string phrase, int startIndex)
    {
        return IndexOf(phrase, startIndex, someText.Length - startIndex);
    }

    public virtual int IndexOf(string phrase, int startIndex, int count)
    {
        return someText.IndexOf(phrase, startIndex, count);
    }
}
```

the class `MyString` provides three overloads for the `IndexOf` method, but two
of them simply call the one with one more parameter; notice that the same rule
applies to class constructors; implement the most complete overload and call that
one from the other overloads using the `this()` operator; also notice that the
parameters with the same name should appear in the same position in all overloads;

> if you also want to allow derived classes to override these methods, define the
> most complete overload as a non-private virtual method that is called by all
> overloads;

### only use optional arguments to replace overloads

`AV1553 only use optional arguments to replace overloads`

**must**: use optional arguments only is to replace with a single method;

```csharp
// before;
public int IndexOf(string phrase)
{
    return IndexOf(phrase, 0);
}

public int IndexOf(string phrase, int startIndex)
{
    return IndexOf(phrase, startIndex, someText.Length - startIndex);
}

public virtual int IndexOf(string phrase, int startIndex, int count)
{
    return someText.IndexOf(phrase, startIndex, count);
}

// after;
public virtual int IndexOf(string phrase, int startIndex = 0, int count = -1)
{
    int length = (count == -1) ? (someText.Length - startIndex) : count;
    return someText.IndexOf(phrase, startIndex, length);
}
```

> if the optional parameter is a reference type then it can only have a default
> value of `null`; but since
> [strings, lists and collections should never be `null`](#strings-or-collections-should-never-be-null)
> you must use overloaded methods instead;
>
> the default values of the optional parameters are stored at the caller side;
> as such, changing the default value without recompiling the calling code will not
> apply the new default value;
>
> when an interface method defines an optional parameter, its default value is
> discarded during overload resolution unless you call the concrete class through
> the interface reference; see [optional argument corner cases, part one](https://blogs.msdn.microsoft.com/ericlippert/2011/05/09/optional-argument-corner-cases-part-one/);

### avoid using named arguments

`AV1555 Avoid using named arguments`

**suggestion**: if you need named arguments to improve the readability of the call
to a method, that method is probably doing too much and should be refactored;

> exception: the only exception where named arguments improve readability is when
> calling a method that has parameters;
>
> ```csharp
> object[] myAttributes = type.GetCustomAttributes(
>   name: typeof(MyAttribute),
>   inherit: false);
> ```

### don't declare signatures with more than 3 parameters

`AV1561 Don't declare signatures with more than 3 parameters`

**must**: to keep constructors, methods, delegates and local functions small and
focused:

- do not use more than three parameters;
- do not use tuple parameters;
- do not return tuples with more than two elements;

> if you want to use more parameters, use a structure or class to pass multiple
> arguments; the fewer the parameters, the easier it is to understand the method;
>
> unit testing a method with many parameters requires many scenarios to test;
>
> exception: a parameter that is a collection of tuples is allowed;

### don't use `ref` or `out` parameters

`AV1562 Don't use ref or out parameters`

**must**: they make code less understandable and might cause people to introduce
bugs; instead, return compound objects or tuples;

> exception: calling & declaring members implementing `TryParse` pattern is allowed;
>
> ```csharp
> bool success = int.TryParse(text, out number);
> ```

### avoid signatures that take a `bool` parameter

`AV1564 Avoid signatures that take a bool parameter`

**recommendation**: avoid signatures that take a `bool` parameter;

```csharp
// invalid;
public Customer CreateCustomer(bool platinumLevel) {}

Customer customer = CreateCustomer(true);
```

often, a method taking such a bool is doing more than one thing and needs to be
refactored into two or more methods;

an alternative solution is to replace the bool with an enumeration;

### don't use parameters as temporary variables

`AV1568 Don't use parameters as temporary variables`

**suggestion**: never use a parameter as a convenient variable for storing
temporary state; even though the type of your temporary variable may be the same,
the name usually does not reflect the purpose of the temporary variable;

### prefer `is` patterns over `as` operations

`AV1570 Prefer is patterns over as operations`

**must**: if you use `as` to safely upcast an interface reference to a certain type,
always verify that the operation does not return `null`; failure to do so may cause
a `NullReferenceException` at a later stage if the object did not implement that
interface; pattern matching syntax prevents this and improves readability;\

```csharp
// invalid;
var remoteUser = user as RemoteUser;
if (remoteUser != null)
{
    // ...
}

// valid;
if (user is RemoteUser remoteUser)
{
    // ...
}
```

### don't comment out code

`AV1575 Don't comment out code`

**must**: never commit code that is commented out; instead, use a work item
tracking system to keep track of some work to be done; nobody knows what to do when
they encounter a block of commented-out code;

## naming

### use american english

`AV1701 Use US English`

**must**: all identifiers (such as types, type members, parameters and variables)
should be named using words from the american english;

- choose easily readable;
- preferably grammatically correct names;
- favor readability over brevity;
- avoid using names that conflict with widely used programming languages keywords;

```csharp
// valid;
int HorizontalAlignment = 0;

public bool CanScrollHorizontally { get; }

// invalid;
int AlignmentHorizontal = 0;

public bool ScrollableX { get; }
```

### use proper casing for language elements

`AV1702 Use proper casing for language elements`

> see also [c# coding standards and naming conventions](https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards%20and%20Naming%20Conventions.md);

**must**: use proper casing for language elements;

> in case of ambiguity, the rule higher in the table wins;

| element                       | casing | prefix | example                  |
| :---------------------------- | :----: | :----: | :----------------------- |
| namespace                     | Pascal |  *no*  | `System.IO`              |
| type parameter                | Pascal |  `T`   | `TView`                  |
| interface                     | Pascal |  `I`   | `ILoggingService`        |
| class                         | Pascal |  *no*  | `MyApplicationException` |
| struct                        | Pascal |  *no*  | `ViewOptions`            |
| enum                          | Pascal |  *no*  | `LogLevel`               |
| enum member                   | Pascal |  *no*  | `FatalError`             |
| resource key                  | Pascal |  *no*  | `SaveButtonTooltipText`  |
| constant field                | Pascal |  *no*  | `MaximumItems`           |
| private constant field        | Pascal |  *no*  | `MinimumItems`           |
| private static readonly field | Pascal |  *no*  | `RedValue`               |
| private field                 | camel  |  `_`   | `_listItem`              |
| non-private field             | Pascal |  *no*  | `MainPanel`              |
| property                      | Pascal |  *no*  | `PanelColor`             |
| event                         | Pascal |  *no*  | `TextboxClick`           |
| method                        | Pascal |  *no*  | `ToString`               |
| local function                | Pascal |  *no*  | `FormatText`             |
| parameter                     | camel  |  *no*  | `typeName`               |
| local variable                | camel  |  *no*  | `valuesList`             |
| tuple element names           | Pascal |  *no*  | `FirstName`              |

### don't include numbers in variables, parameters and type members

`AV1704 Don't include numbers in variables, parameters and type members`

**suggestion**: in most cases they are a lazy excuse for not defining a clear and
intention-revealing name;

### don't prefix fields

`AV1705 Don't prefix fields`

**must**: don't prefix fields; a method in which it is difficult to distinguish
local variables from member fields is generally too big;

> exception: see [prefix column in use proper casing for language elements](#use-proper-casing-for-language-elements);
>
> example: don't use `g_` or `s_` to distinguish static from non-static fields;
> don't `mUserName`, `m_loginTime`;

### don't use abbreviations

`AV1706 Don't use abbreviations`

**recommendation**: don't use abbreviations; avoid single character variable names,
such as `i` or `q`; use `index` or `query` instead;

```csharp
// valid;
ButtonOnClick

// invalid;
BtnOnClick
```

> exceptions: use well-known acronyms and abbreviations that are widely accepted
> or well-known in your work domain; for example: use acronym `UI` instead of
> `UserInterface` and abbreviation `Id` instead of `Identity`;

### name members, parameters, variables according to their meaning, not their type

`AV1707 Name members, parameters and variables according to their meaning and not
their type`

**recommendation**: name members/parameters/variables according to their meaning;

- use functional names;

  > example: `GetLength` is a better name than `GetInt`;

- don't use terms like `Enum`, `Class` or `Struct` in a name;

- identifiers that refer to a collection type should have plural names;

### name types using nouns, noun phrases or adjective phrases

`AV1708 Name types using nouns, noun phrases or adjective phrases`

**recommendation**: name types using nouns, noun phrases or adjective phrases;

```csharp
// valid;
IComponent               // a descriptive noun;
ICustomAttributeProvider // a noun phrase;
IPersistable             // an adjective;

// invalid;
SearchExamination  // a page to search for examinations;
Common             // does not end with a noun;
                   // does not explain its purpose;
SiteSecurity       // the name is technically okay;
                   // but it does not say anything about its purpose;
```

> don't include terms like `Utility` or `Helper` in classes; classes with names like
> that are usually static classes and are introduced without considering
> [object-oriented principles](#avoid-static-classes);

### name generic type parameters with descriptive names

`AV1709 name generic type parameters with descriptive names`

**recommendation**: name generic type parameters with descriptive names;

- always prefix type parameter names with the letter `T`;

- always use a descriptive name unless a single-letter name is completely
  self-explanatory and a longer name would not add value; use the single letter `T`
  as the type parameter in that case;

- consider indicating constraints placed on a type parameter in the name of the parameter;

  > example: a parameter constrained to `ISession` may be called `TSession`;

### don't repeat the name of a class or enumeration in its members

`AV1710 Don't repeat the name of a class or enumeration in its members`

**must**: don't repeat the name of a class or enumeration in its members;

```csharp
class Employee
{
    // invalid;
    static void GetEmployee() { }
    void DeleteEmployee() { }

    // valid;
    static void Get() { }
    void Delete() { }
    void AddNewJob() { }
    void RegisterForMeeting() { }
}
```

### name members similarly to members of related dotnet core classes

`AV1711 Name members similarly to members of related .NET Framework classes`

**suggestion**: dotnet developers are already accustomed to the naming patterns the
framework uses, so following this same pattern helps them find their way in your
classes as well; for instance, if you define a class that behaves like a collection,
provide members like `Add`, `Remove` and `Count` instead of `AddItem`, `Delete` or
`NumberOfItems`;

### avoid short names or names that can be mistaken for other names

`AV1712 Avoid short names or names that can be mistaken for other names`

**must**: although technically correct, following statements can be confusing;

```csharp
bool b001 = (lo == l0) ? (I1 == 11) : (lOl != 101);
```

### properly name properties

`AV1715 Properly name properties`

**recommendation**: properly name properties;

- name properties with nouns, noun phrases or occasionally adjective phrases;

- name boolean properties with an affirmative phrase;

  > example: `CanSeek` instead of `CannotSeek`;

- prefix boolean properties with `Is`, `Has`, `Can`, `Allows` or `Supports`;

- consider giving a property the same name as its type; when you have a property
  that is strongly typed to an enumeration, the name of the property can be the
  same as the name of the enumeration;

  > example, if you have an enumeration named `CacheLevel`, a property that returns
  > one of its values can also be named `CacheLevel`;

### name methods and local functions using verbs or verb-object pairs

`AV1720 Name methods and local functions using verbs or verb-object pairs`

**recommendation**: name a method or local function using a verb like `Show` or
a verb-object pair such as `ShowDialog`; a good name should give a hint on the what
of a member, and if possible, the why;

> don't include `And` in the name of a method or local function; that implies that
> it is doing more than one thing, which violates [the single responsibility principle](#single-responsibility-principle);

### name namespaces using names, layers, verbs and features

`AV1725 Name namespaces using names, layers, verbs and features`

**suggestion**: name namespaces using names, layers, verbs and features;

```csharp
// valid;
AvivaSolutions.Commerce.Web
NHibernate.Extensibility
Microsoft.ServiceModel.WebApi
Microsoft.VisualStudio.Debugging
FluentAssertion.Primitives
```

> never allow namespaces to contain the name of a type, but a noun in its plural
> form (e.g. `Collections`) is usually OK;

### use a verb or verb phrase to name an event

`AV1735 Use a verb or verb phrase to name an event`

**recommended**: name events with a verb or a verb phrase;

```csharp
// valid;
Click
Deleted
Closing
Minimizing
Arriving

public event EventHandler<SearchArgs> Search;
```

### use `-ing` and `-ed` to express pre-events and post-events

`AV1737 Use -ing and -ed to express pre-events and post-events`

**suggestion**: use `-ing` and `-ed` to express pre-events and post-events;

```csharp
// valid;
Closing  // a close event that is raised _before_ a window is closed;
Closed   // a close event that is raised _after_ the window is closed;

// invalid;
BeingDelete
EndDelete

// valid;
Deleting // occurs just before the object is getting deleted;
Delete   // occurs when the object needs to be deleted by the event handler;
Deleted  // occurs when the object is already deleted;
```

### prefix an event handler with `On`

`AV1738 Prefix an event handler with "On"`

**suggestion**: it is good practice to prefix the event handling method with `On`;

> example: a method that handles its own `Closing` event should be named
> `OnClosing` and a method that handles the `Click` event of its `okButton` field
> should be named `OkButtonOnClick`;

### use an underscore for irrelevant lambda parameters

`AV1739 Use an underscore for irrelevant lambda parameters`

**suggestion**: if you use a lambda expression (for instance, to subscribe to an
event) and the actual parameters of the event are irrelevant;

```csharp
button.Click += (_, __) => HandleClick();
```

### group extension methods in a class suffixed with `Extensions`

`AV1745 Group extension methods in a class suffixed with Extensions`

**suggestion**: if the name of an extension method conflicts with another member
or extension method, you must prefix the call with the class name; having them in
a dedicated class with the `Extensions` suffix improves readability;

### postfix asynchronous methods with `Async` or `TaskAsync`

`AV1755 Postfix asynchronous methods with Async or TaskAsync`

**recommended**: the general convention for methods and local functions that return
`Task` or `Task<TResult>` is to postfix them with `Async`; but if such a method
already exists, use `TaskAsync` instead;

## performance

### consider using `Any()` to determine whether an `IEnumerable<T>` is empty

`AV1800 Consider using Any() to determine whether an IEnumerable<T> is empty`

**suggestion**: when a member or local function returns an `IEnumerable<T>` or
other collection class that does not expose a `Count` property, use the `Any()`
extension method rather than `Count()` to determine whether the collection contains
items; if you do use `Count()`, you risk that iterating over the entire collection
might have a significant impact (such as when it really is an `IQueryable<T>` to
a persistent store);

> if you return an `IEnumerable<T>` to prevent changes from calling code consider
> using new read-only classes;

### only use async for low-intensive long-running activities

`AV1820 Only use async for low-intensive long-running activities`

**must**: the usage of `async` won't automatically run something on a worker
thread like `Task.Run` does; it just adds the necessary logic to allow releasing
the current thread, and marshal the result back on that same thread if a
long-running asynchronous operation has completed;

in other words, use `async` only for i/o bound operations;

### prefer `Task.Run` for cpu-intensive activities

`AV1825 Prefer Task.Run for CPU-intensive activities`

**must**: if you do need to execute a cpu bound operation, use `Task.Run` to
offload the work to a thread from the [thread pool](https://en.wikipedia.org/wiki/Thread_pool);
remember that you have to marshal the result back to your main thread manually;

### beware of mixing up `async`/`await` with `Task.Wait`

`AV1830 Beware of mixing up async/await with Task.Wait`

**must**: `await` does not block the current thread but simply instructs the
compiler to generate a state-machine; however, `Task.Wait` blocks the thread and
may even cause deadlocks;

### beware of `async`/`await` deadlocks in single-threaded environments

`AV1835 Beware of async/await deadlocks in single-threaded environments`

**must**: beware of `async`/`await` deadlocks in single-threaded environments;

```csharp
//invalid;

private async Task GetDataAsync()
{
    var result = await MyWebService.GetDataAsync();
    return result.ToString();
}

public ActionResult ActionAsync()
{
    var data = GetDataAsync().Result;

    return View(data);
}
```

`Result` property getter will block until the async operation has completed, but
since an `async` method could automatically marshal the result back to the
original thread (depending on the current `SynchronizationContext` or
`TaskScheduler`) and `asp.net` uses a single-threaded synchronization context,
they'll be waiting on each other;

## framework

### use c# type aliases instead of the types from the `System` namespace

`AV2201 Use C# type aliases instead of the types from the System namespace`

**must**: use c# type aliases instead of the types from the `System` namespace; (AV2201)

```csharp
// invalid;
Object
String
Int32

// valid;
object
string
int
```

> exception: when referring to static members of those types, it is custom to use
> the full [csl](https://simple.wikipedia.org/wiki/Common_Language_Specification)
> name;
>
> ```csharp
> // invalid;
> int.Parse();
>
> // valid;
> Int32.Parse();
> ReadInt32     // members that need to specify
> GetUInt16     // the type they return
> ```

### prefer language syntax over explicit calls to underlying implementations

`AV2202 Prefer language syntax over explicit calls to underlying implementations`

**must**: language syntax makes code more concise; the abstractions make later
refactoring easier (and sometimes allow for extra optimizations);

```csharp
// invalid;
ValueTuple<string, int> tuple = new ValueTuple<string, int>("", 1);

// valid;
(string, int) tuple = ("", 1);

// invalid;
Nullable<DateTime> startDate;

// valid
DateTime? startDate;

// invalid;
if (startDate.HasValue)
{ }

// valid
if (startDate != null)
{ }

// invalid;
if (startDate.HasValue && startDate.Value > DateTime.Now)
{ }

// valid;
if (startDate > DateTime.Now)
{ }

// invalid;
if (tuple1.startTime == tuple2.startTime && tuple1.duration == tuple2.duration)
{ }

// valid;
(DateTime startTime, TimeSpan duration) tuple1 = GetTimeRange();
(DateTime startTime, TimeSpan duration) tuple2 = GetTimeRange();

if (tuple1 == tuple2)
{ }
```

### don't hard-code strings that change based on the deployment

`AV2207 Don't hard-code strings that change based on the deployment`

**suggestion**: don't hard-code strings that change based on the deployment;

> example: connection strings, server addresses, etc.; use `Resources`, the
> `ConnectionStrings` property of the `ConfigurationManager` class; maintain the
> actual values into the builtin or custom configuration;

### build with the highest warning level

`AV2210 Build with the highest warning level`

**must**: configure the development environment to use highest warning level for
the c# compiler, and enable the option treat warnings as errors; this allows the
compiler to enforce the highest possible code quality;

### avoid linq query syntax for simple expressions

`AV2220 Avoid LINQ query syntax for simple expressions`

**suggestion**: avoid linq query syntax for simple expressions;

```csharp
// invalid;
var query = from item in items where item.Length > 0 select item;

// valid;
var query = items.Where(item => item.Length > 0);
```

### use lambda expressions instead of anonymous methods

`AV2221 Use lambda expressions instead of anonymous methods`

**recommendation**: lambda expressions provide a more elegant alternative for
anonymous methods;

```csharp
// invalid;
Customer customer = Array.Find(customers, delegate(Customer customer)
{
    return customer.Name == "Tom";
});

// valid;
Customer customer = Array.Find(customers, customer => customer.Name == "Tom");

// more valid;
var customer = customers.FirstOrDefault(customer => customer.Name == "Tom");
```

### only use the `dynamic` keyword when talking to a dynamic object

`AV2230 Only use the dynamic keyword when talking to a dynamic object`

**must**: the `dynamic` keyword has been introduced for working with dynamic
languages; using it introduces a serious performance bottleneck because the
compiler has to generate some complex reflection code;

use it only for calling methods or members of a dynamically created instance class
as an alternative to `Type.GetProperty()` and `Type.GetMethod()`;

### favor `async`/`await` over `Task` continuations

`AV2235 Favor async/await over Task continuations`

**must**: using the `async`/`await` keywords results in code that can still be
read sequentially and also improves maintainability a lot, even if you need to
chain multiple asynchronous operations;

```csharp
// invalid;
public Task<Data> GetDataAsync()
{
  return MyWebService.FetchDataAsync()
    .ContinueWith(t => new Data(t.Result));
}

// valid;
public async Task<Data> GetDataAsync()
{
  string result = await MyWebService.FetchDataAsync();
  return new Data(result);
}
```

## documentation

### write comments and documentation in american english

`AV2301 Write comments and documentation in US English`

**must**: write comments and documentation in american english;

### document all `public`, `protected` and `internal` types and members

`AV2305 Document all public, protected and internal types and members`

**recommendation**: documenting code allows ide to pop-up the documentation when
your class is used somewhere else; furthermore, by properly documenting your
classes, tools can generate professionally looking class documentation;

### write xml documentation with other developers in mind

`AV2306 Write XML documentation with other developers in mind`

**recommendation**: write the documentation of your type with other developers in
mind; assume they will not have access to the source code and try to explain how
to get the most out of the functionality of your type;

### write msdn-style documentation

`AV2307 Write MSDN-style documentation`

**suggestion**: following the [msdn online](https://msdn.microsoft.com/en-US/)
help style and word choice helps developers find their way through your
documentation more easily;

### avoid inline comments

`AV2310 Avoid inline comments`

**recommendation**: if you feel the need to explain a block of code using a
comment, consider replacing that block with a method with a clear name;

### only write comments to explain complex algorithms or decisions

`AV2316 Only write comments to explain complex algorithms or decisions`

**must**: focus comments on the why and what of a code block and not the how;
avoid explaining the statements in words, but instead help the reader understand
why you chose a certain solution or algorithm and what you are trying to achieve;
if applicable, also mention that you chose an alternative solution because you ran
into a problem with the obvious solution;

### use comments for tracking work to be done later with caution

`AV2318 Don't use comments for tracking work to be done later`

**suggestion**: use comments for tracking work to be done later with caution;
try to fix issues as soon as possible, or delete them, opening new issue on
git repository; avoid such comments on `master` brunch and in release app version;

### use comment patterns

**suggestion**: use comment patterns; [see commenting](#use-comment-patterns);

> [vs code extension](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)
> & [configuration](https://github.com/aaron-bond/better-comments#configuration)
>
> [visual studio extension](https://marketplace.visualstudio.com/items?itemName=OmarRwemi.BetterComments)

comments can be used for better reading of code;
but always put in mind, the best documentation and comments are code;

in following text will be used c# and bash language commentaries as example;

#### important note comment

**starts** with `//!`, `#!`;

**serves** as **dangerous/questionable code** marker;

comments **must** be **deleted** when **no more relevant**;

```csharp
public int Length<T>(List<T> array)
{
    //! throws null exception;
    //!TODO: add null check;
    return array.Count;
}
```

```shell
#! crashes on win platforms;
cd ./out/
```

#### todo comments

**starts** with `//!TODO:`, `#!TODO:`;

**serves** as **issues** marker;

comments **deletes when resolved or issue on topic is open**

```csharp
// TODO:FIXME: change method name to `GetLength()` to follow guidelines;
public static int Length<T>(this List<T> array)
{
    // TODO: make `IEnumerable<T>`, `IList<T>` versions;
    return array?.Count ?? 0;
}
```

```shell
#!TODO: add win script;
#!TODO:FIXME: crashes on win platform;
cd ./out/
```

#### fixme comments

**starts** with `//!TODO:FIXME:`, `#!TODO:FIXME:`

see [todo comments](#todo-comments);

#### temporary solutions comments

**starts** with `//!tmp`, `#!tmp`;

**ends** with `//!tmp end`, `#!tmp end`;

**serves** as **temporary code** marker;

comments **must not** be **committed**, **must always** be **deleted**,
are **forbidden in release** code;

```csharp
public int Length<T>(List<T> array)
{
    //!tmp;
    return (array ?? new List<T>()).Count;
    //!tmp end;
}
```

```shell
#!tmp; some debug info;
bash --version
lsb_release -a
#!tmp end;
```

#### issue workaround comments

**starts** with `//!tmp issue #{issue}`, `#!tmp issue #{issue}`,

> `{issue}` is number of related issue;

**ends** with `//!tmp issue end`, `#!tmp issue end`;

**serves** as **code for workaround issue** marker;

comments **must** be **deleted** when **relevant issue resolved**,
**cannot** be present **in stable app version**;

```csharp
public int Length<T>(List<T> array)
{
    //!tmp issue #4;
    //!issue https://github.com/vulmy/server/issues/4
    return (array ?? new List<T>()).Count;
    //!tmp issue end;
}
```

```shell
#!tmp issue #42; crashes on alpine;
apk add --update bash
bash --version
# lsb_release -a
#!tmp issue end;
```

#### code notice comments

**starts** with `//?`, `#?`

**serves** as **non-obvious solution explanation**;

comments **must** be **deletes** when **no longer relevant**

```csharp
public static int Length<T>(this List<T> array)
{
    //? extension method can have source object null;
    //? see more at https://stackoverflow.com/a/847211 ;
    return array?.Count ?? 0;
}
```

```shell
#? install bash cause `bash --version` crashes on CI alpine;
apk add --update bash
bash --version
```

#### documentation comments

**starts** with `///`, `##`;

**serves** as **documentation** commentaries;

comments **can** be **deleted** when **no longer relevant**

> example: when method/class became private

```csharp
/// <summary>same as <see cref="IList<T>.Count" /> with null check</summary>
/// <remarks>uses <see cref="IList<T>.Count" /></remarks>
/// <returns>number of collection elements or 0 if collection is null</returns>
public static int Length<T>(this List<T> array)
    => array?.Count ?? 0;
```

```shell
#!/bin/bash

## displays system information;
##  no arguments required;

lsb_release -a
bash --version
dotnet --version
```

#### any commented out code

comments **must** be **deleted**;

```csharp
/*
public static int Length<T>(this List<T> array)
    => array?.Count ?? 0;
*/

// public const int Answer = 42;
```

```shell
# bash --version
## dotnet --version
```

#### any other comments

comments **must** be **deleted**;

```csharp
// length? not count???
public static int Length<T>(this List<T> array)
    // wtf?, meaningless method?
    => array?.Count ?? 0;
```

```shell
# separate shell script for this???
dotnet --version
```

## layout

### use a common layout

`AV2400 Use a common layout`

**must**: use a common layout;

- keep the length of each line under 130 characters;

- use an indentation of 4 spaces, and don't use tabs;

- keep one space between keywords like `if` and the expression, but don't add
  spaces after `(` and before `)`

  > example: `if (condition == null)`;

- add a space around operators like `+`, `-`, `==` etc.;

- always put opening and closing curly braces on a new line;

- don't indent object/collection initializers and initialize each property on a
  new line;

  ```csharp
  var dto = new ConsumerDto
  {
      Id = 123,
      Name = "Microsoft",
      PartnerShip = PartnerShip.Gold,
      ShoppingCart =
      {
          ["VisualStudio"] = 1
      }
  };
  ```

- don't indent lambda statement blocks;

  ```csharp
  methodThatTakesAnAction.Do(x =>
  {
      // ...
  }
  ```

- keep expression-bodied-members on one line; break long lines after arrow sign;

  ```csharp
  private string GetLongText =>
      "ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC";
  ```

- put the entire linq statement on one line, or start each keyword at the same
  indentation;

  ```csharp
  var query = from product in products where product.Price > 10 select product;

  var query =
      from product in products
      where product.Price > 10
      select product;
  ```

- start the linq statement with all the `from` expressions and don't interweave
  them with restrictions;

- add parentheses around every binary expression, but don't add parentheses around
  unary expressions;

  > example: `if (!string.IsNullOrEmpty(str) && (str != "new"))`;

- add an empty line between multi-line statements, between multi-line members,
  after the closing curly braces, between unrelated code blocks, around the
  `#region` keyword, and between the `using` statements of different root
  namespaces;

### order and group namespaces

`AV2402 Order and group namespaces according to the company`

**suggestion**: order and group namespaces

```csharp
// builtin framework namespaces;
using System;
using System.Collections.Generic;
using System.XML;

// third party external namespaces;
using NLog;

// external project namespaces;
using Vulmy.Server.Api;

// internal project namespaces;
using Vulmy.Server.Api.Test.DataTypes;

namespace Vulmy.Server.Api.Test // test project;
```

> using static directives and using alias directives should be written below
> regular using directives; always place these directives at the top of the file,
> before any namespace declarations not inside them;

### place members in a well-defined order

`AV2406 Place members in a well-defined order`

**must**: maintaining a common order allows other team members to find their way
in your code more easily; in general, a source file should be readable from top to
bottom, as if reading a book, to prevent readers from having to browse up and down
through the code file;

1. private fields and constants; *in a region*;
1. public constants
1. public static read-only fields;
1. factory methods;
1. constructors and the finalizer;
1. events;
1. public properties;
1. other methods and private properties in calling order;

> declare local functions at the bottom of their containing method bodies after
> all executable code;

### be reluctant with `#region`

`AV2407 Be reluctant with #region`

**must**: be reluctant with `#region`; regions can be helpful, but can also hide
the main purpose of a class; therefore, use `#region` only for following;

- private fields and constants; *preferably in a private definitions region*;
- nested classes;
- interface implementations; *if the interface is not the main purpose of class*;

### use expression-bodied members appropriately

`AV2410 Use expression-bodied members appropriately`

**must**: favor expression-bodied member syntax over regular member syntax only if;

- the body consists of a single statement `and`;
- the body fits on a single line;

## link & articles

- `Code Complete: A Practical Handbook of Software Construction` by Steve McConnel;

- `The Art of Agile Development` by James Shore;

- `Applying Domain-Driven Design and Patterns: With Examples in C# and .NET`
  by Jimmy Nilsson;

- [LINQ Framework Design Guidelines](https://blogs.msdn.microsoft.com/mirceat/2008/03/12/linq-framework-design-guidelines/);

- [Best Practices for c# async/await](https://msdn.microsoft.com/en-us/magazine/jj991977.aspx);
