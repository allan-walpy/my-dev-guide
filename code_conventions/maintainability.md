# maintainability

## methods should not exceed 7 statements

`AV1500 Methods should not exceed 7 statements`

**must**: a method that requires more than 7 statements is simply doing too much
or has too many responsibilities; it also requires the human mind to analyze the
exact statements to understand what the code is doing; break it down into
multiple small and focused methods with self-explaining names, but make sure the
high-level algorithm is still clear;

## make all members private and types internal sealed by default

`AV1501 Make all members private and types internal sealed by default`

**must**: to make a more conscious decision on which members to make available to
other classes, first restrict the scope as much as possible; then carefully
decide what to expose as a public member or type;

## avoid conditions with double negatives

`AV1502 Avoid conditions with double negatives`

```csharp
bool hasOrders = !customer.HasNoOrders;
```

**recommendation**: although a property like `customer.HasNoOrders` makes sense,
avoid using it in a negative condition; double negatives are more difficult to
grasp than simple expressions;

## name assemblies after their contained namespace

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

## name a source file to the type it contains

`AV1506 Name a source file to the type it contains`

**suggestion**: use `PascalCasing` to name the file and don't use underscores;
don't include (the number of) generic type parameters in the file name;

## limit the contents of a source code file to one type

`AV1507 Limit the contents of a source code file to one type`

**suggestion**: limit the contents of a source code file to one type;

> exceptions:
>
> nested types should be part of the same file;
>
> types that only differ by their number of generic type parameters should be
> part of the same file;

## name a source file to the logical function of the partial type

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

## use using statements instead of fully qualified type names

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

## don't use magic numbers

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

## only use var when the type is very obvious

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

## declare and initialize variables as late as possible

`AV1521 Declare and initialize variables as late as possible`

**recommendation**: avoid the c/visual basic/pascal styles where all variables
have to be defined at the beginning of a block, but rather define and initialize
each variable at the point where it is needed;

## assign each variable in a separate statement

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

## favor object and collection initializers over separate statements

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

## don't make explicit comparisons to true or false

`AV1525 Don't make explicit comparisons to true or false`

**must**: it is usually bad style to compare a bool-type expression to `true` or
`false`;

```csharp
// invalid;
while (condition == false) { }
while (condition != true) { }

// valid;
while (condition) { }
```

## don't change a loop variable inside a for loop

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

## Avoid nested loops

`AV1532 Avoid nested loops`

**recommendation**: a method that nests loops is more difficult to understand than
one with only a single loop. In fact, in most cases nested loops can be replaced
with a much simpler linq query that uses the `from` keyword twice or more to join
the data;

## always add a block after the keywords if, else, do, while, for, foreach, case

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

## always add a default block after the last case in a switch statement

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

## finish every if-else-if statement with an else clause

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

## be reluctant with multiple return statements

`AV1540 Be reluctant with multiple return statements`

**recommendation**: one entry, one exit is a sound principle and keeps control
flow readable; however, if the method body is very small and complies with
[methods should not exceed 7 statements](#methods-should-not-exceed-7-statements)
then multiple return statements may actually improve readability over some central
boolean flag that is updated at various points;

## don't use an if-else construct instead of a simple conditional assignment

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

## encapsulate complex expressions in a property, method or local function

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

## call the more overloaded method from other overloads

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

## only use optional arguments to replace overloads

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
> [strings, lists and collections should never be `null`](./member_design.md#strings-or-collections-should-never-be-null)
> you must use overloaded methods instead;
>
> the default values of the optional parameters are stored at the caller side;
> as such, changing the default value without recompiling the calling code will not
> apply the new default value;
>
> when an interface method defines an optional parameter, its default value is
> discarded during overload resolution unless you call the concrete class through
> the interface reference; see [optional argument corner cases, part one](https://blogs.msdn.microsoft.com/ericlippert/2011/05/09/optional-argument-corner-cases-part-one/);

## avoid using named arguments

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

## don't declare signatures with more than 3 parameters

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

## don't use ref or out parameters

`AV1562 Don't use ref or out parameters`

**must**: they make code less understandable and might cause people to introduce
bugs; instead, return compound objects or tuples;

> exception: calling & declaring members implementing `TryParse` pattern is allowed;
>
> ```csharp
> bool success = int.TryParse(text, out number);
> ```

## avoid signatures that take a bool parameter

`AV1564 Avoid signatures that take a bool parameter`

**recommendation**: avoid signatures that take a bool parameter;

```csharp
// invalid;
public Customer CreateCustomer(bool platinumLevel) {}

Customer customer = CreateCustomer(true);
```

often, a method taking such a bool is doing more than one thing and needs to be
refactored into two or more methods;

an alternative solution is to replace the bool with an enumeration;

## don't use parameters as temporary variables

`AV1568 Don't use parameters as temporary variables`

**suggestion**: never use a parameter as a convenient variable for storing
temporary state; even though the type of your temporary variable may be the same,
the name usually does not reflect the purpose of the temporary variable;

## prefer is patterns over as operations

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

## don't comment out code

`AV1575 Don't comment out code`

**must**: never commit code that is commented out; instead, use a work item
tracking system to keep track of some work to be done; nobody knows what to do when
they encounter a block of commented-out code;
