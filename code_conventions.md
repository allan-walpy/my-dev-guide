# Code Conventions

## Basic Principles

### Least Surprise

Choose a solution that everyone can understand, and that keeps them on right track

### Keep It Simple Stupid

The simplest solution is more than sufficient

### You Ain't Gonna Need It

Create a solution for the problem at hand,
not for the ones you think may happen later on

### Don't Repeat Yourself

Avoid duplication within a component, a source control repository
or a bounded context

### The 4 principles of object oriented programming

#### Encapsulation

Bundling of data with the methods that operate on that data

#### Abstraction

Keeping common features or attributes to various concrete objects

#### Inheritance

Basing an class upon another class retaining similar implementation

#### Polymorphism

Provision of a single interface to entities of different types

## Class Design

### Single Responsibility Principle

A class, interface or method should have a single purpose
within the system it functions in

### Interface Segregation Principle

Interfaces should have a name that clearly explains their purpose
or role in the system

> **Do not** combine many vaguely related members on the same
> interface just because they were all on the same class

### Useful Constructors

Only create a constructor that returns a useful object.
There should be no need to set additional properties
before the object can be used for whatever purpose it was designed

> **NB!** If your constructor needs more than 3 parameters, your class
> might have too much responsibility

### Use interface to decouple classes from each other

Interfaces are a very effective mechanism
for decoupling classes from each other

> **NB!** Using a dependency injections you can centralize
> the choice of which class is used whenever a specific interface
> is requested

### Use interface class rather than base class

Use an interface rather than a base class to support multiply implementations

> **NB!** You may implement an abstract default implementation of
> interface that can serve as a starting point

### Avoid static classes

Static classes very often lead to badly designed code.
They are also very difficult to test in isolation

> **Exception** are extension method containers

### Liskov Substitution Principle

It should be possible to treat a derived object as if it were a base
class object. You should be able to use a reference to an object of
a derived class wherever a reference to its base class object is used
without knowing the specific derived class

### Avoid exposing the other objects an object depends on

> In simple words: `someObject.SomeProperty.GetChild().Foo();`

An object should not expose any other classes it depends on
because callers may misuse that exposed property or method
to access the object behind it

> **NB!** Using a class that is designed using
> the Fluent Interface pattern seems to violate this rule,
> but it is simply returning itself so that method chaining is allowed

## Member Design

### Allow properties to be set in any order

There should not be a difference between first setting
property `DataSource` and then `DataMember` or vice-versa.

### Use a method instead of a property

- If the work is more complicated than setting a field value

- If it represents a conversion such as the `Object.ToString` method

- If it returns a different result each time it is called,
  even if the arguments didn't change

  > For example, the `NewGuid` method returns a different value
  > each time me it is called

- If the operation causes a side effect such as changing some
  internal state not directly related to the property

### Do not use mutually exclusive properties

### A property, method or local function must do one thing

### Do not expose stateful objects through static members

> A stateful object is an object that contains many properties
> and lots of behavior behind it.

If you expose such an object through a static property or method
of some other object, it will be very difficult
to refactor or unit test a class that relies on such a stateful object.

### Return an built in interfaces for collections

Return an built in interfaces for collections, such as
`IEnumerable<T>`, `ICollection<T>` or `IList<T>`,
instead of some concrete collection class

> **Exceptions**
>
> - Readonly collection `IReadOnlyCollection<T>`, `IReadOnlyList<T>`,
>   `IReadOnlyDictionary<TKey, TValue>`
> - Immutable collections `ImmutableArray<T>`, `ImmutableList<T>`
>   and `ImmutableDictionary<TKey, TValue>`

### Strings or collections should never be `null`

Always return an empty collection or an empty string instead of
a null reference. This also prevents cluttering your code base
with additional checks for null, or even `String.IsNullOrEmpty()`.

### Define parameters as specific as possible

Define parameters as specific as needed and don't take a container object instead

> **NB!** `Do not ship the truck if you only need a package`

### Use domain-specific value types, not primitives

Instead of using strings, integers and decimals for representing
domain-specific types such as an ISBN number, an email address
or amount of money, consider creating dedicated value objects
that wrap both the data and the validation rules that apply to it

## Other Design

### Events

- When dealing with events always use delegate `EventHandler`
  or its derivatives

- Always check an event handler delegate for null

  > Before invoking, always make sure that the delegate
  > list represented by the event variable is
  > not null.
  >
  >```c#
  > event EventHandler<NotifyEventArgs> Notify;
  >
  > protected virtual void OnNotify(NotifyEventArgs args)
  > {
  >     Notify?.Invoke(this, args);
  > }
  >```

#### Event parameters

- Event parameters must always be `EventArgs` or its derivative
- Event parameters always should be called `e`
- `sender` must always be `object` type

### Throw exceptions instead of returning status value

Structured exception handling has been introduced to allow you
to throw exceptions and catch or replace them at a higher layer.
In most systems it is quite common to throw exceptions whenever
an unexpected situation occurs.

### Provide a rich and meaningful exception message text

The message should explain the cause of the exception,
and clearly describe what needs to be done to avoid the exception.

### Throw the most specific exception that is appropriate

If a method receives a `null` argument, it should throw
`ArgumentNullException` instead of its base type
`ArgumentException` or `ApplicationException`.

### Don't swallow errors by catching generic exceptions

Avoid swallowing errors by catching non-specific exceptions,
such as `Exception`, `SystemException` and so on, in application code.
Only in top-level code, such as a last-chance exception handler, you
should catch a non-specific exception for logging purposes and a
graceful shutdown of the application

### Properly handle exceptions in asynchronous code

When throwing or handling exceptions in code that uses
`async`/`await` or a `Task` remember the following two rules:

- Exceptions that occur within an `async`/`await` block
  and inside a `Task`'s action are propagated to the awaiter

- Exceptions that occur in the code preceding
  the asynchronous block are propagated to the caller

### Do not use `this` and `base` prefixes unless it is required

In a class hierarchy, it is not necessary to know at which level
a member is declared to use it

## Maintainability Guidelines

### Methods should not exceed 7 statements

A method that requires more than 7 statements is simply doing
too much or has too many responsibilities. Break it down into multiple
small and focused methods with self-explaining names, but
make sure the high-level algorithm is still clear.

### Make members private, types internal sealed as default

To make a more conscious decision on which members to make available
to other classes, first restrict the scope as much as possible.
Then carefully decide what to expose as a public member or type

### Avoid conditions with double negatives

Although a property like `customer.HasNoOrders` makes sense,
avoid using it in a negative condition like this:

```c#
bool hasOrders = !customer.HasNoOrders;
```

### Name assemblies after their contained namespace

All DLLs should be named according to the pattern
`Product.Component.dll` where `Product` refers to your product's name
and `Component` contains one or more dot-separated clauses.

> **Example** `Vulmy.Client.Web.dll`

### Name a source file to the type it contains

Use Pascal casing to name the file and don't use underscores.
Don't include generic type parameters in the file name

### Limit the contents of a source code file to one type

> **Exceptions**
>
> - Nested types should be part of the same file
>
> - Types that only differ by their number of generic type parameters
>   should be part of the same file

### Name partial class file according it's logical function

When using partial types and allocating a part per file,
name each file after the logical part that part plays.

```c#
// MyClass.cs file;
public partial class MyClass
{...}

// MyClass.Designer.cs file;
partial class MyClass
{...}
```

### Use `using` statements instead of fully qualified type names

Limit usage of fully qualified type names to prevent name clashing.

```c#
// avoid;>   >
var list = new System.Collections.Generic.List<string>();

// valid;
using System.Collections.Generic;

var list = new List<string>();
```

If you do need to prevent name clashing, use a using directive
to assign an alias:

```c#
using Label = System.Web.UI.WebControls.Label;
```

### Do not use magic numbers

Don't use literal values, either numeric or strings, in your code,
other than to define symbolic constants.

### Only use var when the type is very obvious

Only use var as the result of a LINQ query, or if the type is very
obvious from the same statement and using it would improve readability

```c#
// not valid;
var item = 3;
var myFoo = MyFactoryMethod.Create("arg");

// valid;
var query = from order
    in orders
    where order.Items > 10
        and order.TotalValue > 1000;
var repository = new RepositoryFactory.Get();
var list = new ReadOnlyCollection();
```

### Declare and initialize variables as late as possible

Define and initialize each variable at the point where it is needed.

### Don't make explicit comparisons to true or false

It is usually bad style to compare a bool-type expression to true or false.

```c#
// not valid;
if (b == true)
{
    var a = (b == false);
}

// valid;
if (b)
{
    var a = !b;
}
```

### Do not change a loop variable inside a `for` loop

```c#
// not valid;
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

### Avoid nested loops

A method that nests loops is more difficult to understand
than one with only a single loop

### Always add a block after the keywords

Such as `if`, `else`, `do`, `while`, `for`, `foreach` and `case`

### Add a `default` block in the end of `switch` statement

Add a descriptive comment if the default block is supposed
to be empty. If that block is not supposed to be reached
throw an `InvalidOperationException`

### Encapsulate complex expressions

Encapsulate complex expressions in a property, method or local function

```c#
// not valid;
if (member.HidesBaseClassMember && (member.NodeType != NodeType.InstanceInitializer))
{
    SomeMethodCall();
}

// valid;
if (NonConstructorMemberUsesNewKeyword(member))
{
    SomeMethodCall();
}
```

### Call the more overloaded method from other overloads

This guideline only applies to overloads that are intended
to provide optional arguments

```c#
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
```

### Only use optional arguments to replace overloads

```c#
public virtual int IndexOf(string phrase, int startIndex = 0, int count = -1)
{
    int length = (count == -1) ? (someText.Length - startIndex) : count;
    return someText.IndexOf(phrase, startIndex, length);
}
```

### Avoid using named arguments

The only exception where named arguments improve readability

```c#
// not valid
object[] a = b.GetAttributes(type: typeof(MyAttribute));

// valid;
object[] a = b.GetAttributes(typeof(MyAttribute));

// also valid;
object[] a = b.GetAttributes(typeof(MyAttribute), inherit: false);
```

### Do not use `ref` or `out` parameters

They make code less understandable. Instead, return compound objects or tuples.

### Do not use parameters as temporary variables

### Prefer `is` patterns over `as` operations

```c#
// not valid;
var remoteUser = user as RemoteUser;
if (remoteUser != null)
{
    // some code;
}

// valid
if (user is RemoteUser remoteUser)
{
    // some code;
}
```

## Documentation and Comments Guideline

> for more relevant info see [contribution guidance](./documentation.md)

|     type name | condition              |      serves      |   when deletes   |
| ------------: | :--------------------- | :--------------: | :--------------: |
|     commented | starts `//`            |     comment      |      always      |
|      out code |                        | alternative code |                  |
|               |                        |                  |                  |
|     important | starts `//!`           |    warning of    |    no longer     |
|       comment |                        |  dangerous code  |      actual      |
|               |                        |                  |                  |
|     temporary | starts `//!tmp`        |    temporary     |      always      |
|          code | ends `//!tmp end;`     |  debugging code  |                  |
|               |                        |                  |                  |
|     temporary | starts `//!tmp issue`  |    temporary     | when referenced  |
|          code | ends `//!tmp end;`     | workaround code  |   issue solved   |
|               |                        |                  |                  |
|      question | starts `//?`           |   explanation    | no longer actual |
|       comment |                        |     of code      |   issue opened   |
|               |                        |                  |                  |
|     commented | starts `///`;          |     comment      |    on private    |
|          code |                        | alternative code |     members      |
|               |                        |                  |                  |
|     commented | starts `/*`            |     comment      |      always      |
|    code block | ends `*/`              | alternative code |                  |
|               |                        |                  |                  |
| documentation | starts `///`           |     `*.xml`      |    on private    |
|               | contains xml tags      |  documentation   |     members      |
|               |                        |                  |                  |
|          TODO | starts `//!TODO:`      |   quick notice   | no longer actual |
|       comment |                        |    for future    |   issue opened   |
|               |                        |                  |                  |
|          TODO | starts `//!TODO:TEST`  | test requirement |   same as TODO   |
|  test comment |                        |      notice      |    appliance     |
|               |                        |                  |                  |
|         FIXME | starts `//!TODO:FIXME` |   quick notice   |   same as TODO   |
|       comment |                        |   of bad code    |    appliance     |
|               |                        |                  |                  |
|   wrong FIXME | starts with `//FIXME:` |   quick notice   |    change on     |
|       comment |                        |   of bad code    | `//!TODO:FIXME`  |
|               |                        |                  |                  |
|         other | any other              |  coder comments  |      always      |
|      comments | comments               |    jokes etc.    |                  |

### Use comments for tracking work to be done later

Use `TODO:` and `TODO:FIXME:` formats - codacity easily track them
or they can be easily searched, they are easy to search in vs code
and others IDE, [can be highlighted](#Documentation-and-Comments-Guideline)

### Language

All documentation and commenting goes on GB English.

### Document `public`, `protected` and `internal`

Document all `public`, `protected` and `internal` types and members

### Avoid inline comments

If you feel the need to explain a block of code using a comment,
consider replacing that block with a method with a clear name

### Use comments to explain complex/non obvious decisions

Help the reader understand why you chose a certain solution or
algorithm and what you are trying to achieve.

## Naming Guidelines

| subject                       |  case  | prefix | example                  |
| :---------------------------- | :----: | :----: | :----------------------- |
| Namespace                     | Pascal |        | Microsoft.AspNetCore.Mvc |
| Class, struct                 | Pascal |        | MyClass                  |
| Method                        | Pascal |        | SetValue                 |
| Local variable                | camel  |        | oldValue                 |
| Method argument               | camel  |        | newValue                 |
| Constant field                | Pascal |        | MaximumItems             |
| Private constant field        | Pascal |        | MinimumItems             |
| Private static readonly field | Pascal |        | DefaultResponse          |
| Private field                 | camel  |  `_`   | _listItem                |
| Property                      | Pascal |        | BackColor                |
| Non-private field             | Pascal |        | MainPanel                |
| Interface                     | Pascal |  `I`   | IDatabaseService         |
| Enum                          | Pascal |        | ErrorLevel               |
| Enum member                   | Pascal |        | FatalError               |
| Resource key                  | Pascal |        | SaveButtonTooltipText    |
| Event                         | Pascal |        | OnClick                  |
| Local function                | Pascal |        | FormatText               |
| Parameter                     | camel  |        | typeName                 |

### Avoid using numbers in names

```c#
// acceptable;
public Max(item1, item2);

// valid;
public Max(firstItem, secondItem);
```

### Do not prefix names (excluding prefixes) to avoid collisions

```c#
// not valid;
private NumberComparer comparer;

public Max(firstItem, secondItem, _comparer);

// valid;
private NumberComparer comparer;

public Max(firstItem, secondItem, customComparer);
```

### Do not use self-invented abbreviations

```c#
// valid;
var ftp, html, id, max, min;

// acceptable;
var auth;

// not valid;
var theWtf, onBtnClick;
```

### Do not use type based postfix and prefixes

Do not use type based postfix and prefixes
like `Enum`, `Class`, `Strict`, unless it is required

```c#
// valid;
public class LoginController : BaseController {}
public class ApiResourceAttribute : AuthorizeAttribute {}

// not valid;
public enum ErrorStateEnum {}
public struct ResponseStruct {}
```

### Do name members, variables according to their meaning

Do not name according their type

```c#
// valid;
public interface IServiceProvider {}
public class ApiResource {}
public class CommonServiceProvider {}
public class TokenBuilder {}
public struct UserExtensions {}

// acceptable;
public class TokenHelper {}
public class UserUtility {}

// not valid;
public class Common {}
public struct SiteSecurity {}
```

### Do not repeat the name of a class or enumeration

```c#
// not valid;
public class Employee
{
    public static GetEmployee();
}

// valid;
public class Employee
{
    public static Get();
}
```

### Name methods using verbs or verb-object pairs

### Name namespaces using names, layers, verbs and features

```c#
// valid;
using Microsoft.ServiceModel.WebApi;
using Microsoft.VisualStudio.Debugging;
using AvivaSolutions.Commerce.Web;
using FluentAssertion.Primitives;
using CaliburnMicro.Extensions;
```

### Group extension methods in a class suffixed with `Extensions`

### Postfix asynchronous methods with `Async` or `TaskAsync`

The general convention for methods and local functions
that return `Task` or `Task<TResult>` is to postfix them with `Async`.

## Layout Guidelines

### Common layout

|                   |                   |
| :---------------- | :---------------: |
| line: indentation | 4 spaces, no tabs |
| line: max length  |  100 characters   |
| line: ending EOL  |  LF, unix style   |
| file: encoding    | utf 8 without BOM |
| file: max size    |     200 lines     |

### Keep space between keywords and expressions

Keep space between keywords and expressions but use no spaces after
`(` and before `)`

```c#
    if (condition == null)
```

### Add a space around operators like `+`, `-`, `==` abd others

```c#
a = 1 + 2;
```

### Always put opening and closing curly braces on a new line

```c#
public class MyClass
{
    // code;
}
```

> **Exception** if body is absent or is auto property
>
> ```c#
> // valid;
> public Name { get; set; }
>
> // valid;
> public class MyClass
> { }
>
> // not valid;
> public class MyClass {
> }
>
> // not valid;
> public MyClass() { }
>
> // not valid;
> public void DoNothing() { }
> ```

### Do not indent object/collection initializers

Initialize each property on a new line

```c#
var blackCat = new Cat
{
    Color = Colors.Black,
    Name = "Felix",
    ShoppingCart =
    {
        ["Mouse"] = 1
    }
};
```

#### Do not indent lambda statement block

```c#
methodThatTakesAnAction.Do(x =>
{
    // do something like this
}
```

#### LINQ Statement

Put the entire LINQ statement on one line, or start each keyword
at the same indentation

```c#
var inlineQuery = from product in products where product.Price > 10 select product;

var blockQuery =
    from product in products
    where product.Price > 10
    select product;
```

#### Add parentheses to binary expressions, but not unary ones

```c#
if (!string.IsNullOrEmpty(str) && (str != "new"))
```

#### Add an empty line

Add an empty line between multi-line statements, between multi-line
members, after the closing curly braces, between unrelated code blocks,
around the `#region` keyword and between the `using` statements
according to the grouping policy in project

### Namespace grouping policy

Namespaces must be grouped into two, they separates from other
with one empty line

```c#
using Group1.Smth;
using Group1.Smth.Tool;

using Group2.Smth;
using Group2.SmthTool;
```

#### First namespace group

> There used to be separation between system + microsoft and third
> party namespaces, but on practice microsoft official packages may
> include third party packages,
> [for example `Newtonsoft.Json` is part of `Microsoft.Extensions.Configuration.Json`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json/)

System, microsoft and third-party packets. They go in this order:

- `System.*` in alphabet order;
- `Microsoft.*` in alphabet order;
- others in alphabet order;

```c#
using System;
using System.IO;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Configuration.Json;
using Newtonsoft.Json;
using Nlog;
```

#### Second namespace group

Inside solution dependencies in alphabet order

```c#
using Vulmy.Client;
using Vulmy.Server;
using Vulmy.Server.Services;
using Vulmy.Test.Instances;
```

### Members order

- Public constants and static readonly fields
- Private constants and static readonly fields
- Private fields
- Public fields
- Constructors and destructors
- Public properties
- Events
- Other methods in calling order

### Use expression bodied members

Favor expression-bodied member syntax over regular member syntax only
when the body consists of a single statement and fits on a single line.

## Performance Guidelines

### Use `Any()` to determine whether an `IEnumerable<T>` is empty

If you do use `Count()`, you risk iterating over the entire collection

### Only use async for low-intensive long-running activities

Use async only for I/O bound operators

### Beware of mixing up `async`/`await` with `Task.Wait`

`await` does not block the current thread but simply instructs
the compiler to generate a state-machine. However, `Task.Wait` blocks
the thread and may even cause deadlocks.

## Framework Guidelines

### Use C# type aliases instead of System namespace ones

Use `object`, `int`, `string`

> **Exception** When referring to static members of those types use
> the full CLS name, e.g. `Int32.Parse()`, `String.Format()`.
> The same applies to members that need to specify the type they
> return, e.g. `ReadInt32`, `GetUInt16`.

### Prefer language syntax over explicit calls

```c#
// good;
(string, int) tuple = ("", 1);

// bad;
ValueTuple<string, int> tuple = new ValueTuple<string, int>("", 1);

// good;
DateTime? startDate;

// bad;
Nullable<DateTime> startDate;

// good;
if (startDate != null)

// bad;
if (startDate.HasValue)

// good;
if (startDate > DateTime.Now)

// bad;
if (startDate.HasValue && startDate.Value > DateTime.Now)

// good;
(DateTime startTime, TimeSpan duration) tuple1 = GetTimeRange();
(DateTime startTime, TimeSpan duration) tuple2 = GetTimeRange();
if (tuple1 == tuple2)

// bad;
if (tuple1.startTime == tuple2.startTime && tuple1.duration == tuple2.duration)
```

### Do not hard-code strings that change based on the deployment

Examples include connection strings, server addresses, etc.
Use `./config/main.json` file of repository

### Use lambda expressions instead of anonymous methods

```c#
// not valid;
Customer customer = Array.Find(customers, delegate(Customer customer)
{
    return customer.Name == "Tom";
});

// valid;
Customer customer = Array.Find(customers, customer => customer.Name == "Tom");

// valid;
var customer = customers.Where(customer => customer.Name == "Tom").FirstOrDefault();
```

### Only use `dynamic` when talking to a dynamic object

Using it introduces a serious performance bottleneck because
the compiler has to generate some complex Reflection code.

Use it only for calling methods or members of a dynamically created
instance class as an alternative to `Type.GetProperty()`
and `Type.GetMethod()`

### Favor `async`/`await` over `Task` continuations

```c#
// not valid;
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

## Licence notice

This code conventions are a modification of [dennisdoomen/CSharpGuidelines](https://github.com/dennisdoomen/CSharpGuidelines/releases/tag/5.3.0);
