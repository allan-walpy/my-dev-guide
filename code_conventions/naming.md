# naming

## use american english

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

## use proper casing for language elements

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

## don't include numbers in variables, parameters and type members

`AV1704 Don't include numbers in variables, parameters and type members`

**suggestion**: in most cases they are a lazy excuse for not defining a clear and
intention-revealing name;

## don't prefix fields

`AV1705 Don't prefix fields`

**must**: don't prefix fields; a method in which it is difficult to distinguish
local variables from member fields is generally too big;

> exception: see [prefix column in use proper casing for language elements](#use-proper-casing-for-language-elements);
>
> example: don't use `g_` or `s_` to distinguish static from non-static fields;
> don't `mUserName`, `m_loginTime`;

## don't use abbreviations

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

## name members, parameters and variables according to their meaning, not their type

`AV1707 Name members, parameters and variables according to their meaning and not
their type`

**recommendation**: name members/parameters/variables according to their meaning;

- use functional names;

  > example: `GetLength` is a better name than `GetInt`;

- don't use terms like `Enum`, `Class` or `Struct` in a name;

- identifiers that refer to a collection type should have plural names;

## name types using nouns, noun phrases or adjective phrases

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
> [object-oriented principles](./class_design.md#avoid-static-classes);

## name generic type parameters with descriptive names

`AV1709 name generic type parameters with descriptive names`

**recommendation**: name generic type parameters with descriptive names;

- always prefix type parameter names with the letter `T`;

- always use a descriptive name unless a single-letter name is completely
  self-explanatory and a longer name would not add value; use the single letter `T`
  as the type parameter in that case;

- consider indicating constraints placed on a type parameter in the name of the parameter;

  > example: a parameter constrained to `ISession` may be called `TSession`;

## don't repeat the name of a class or enumeration in its members

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

## name members similarly to members of related dot net core classes

`AV1711 Name members similarly to members of related .NET Framework classes`

**suggestion**: dotnet developers are already accustomed to the naming patterns the
framework uses, so following this same pattern helps them find their way in your
classes as well; for instance, if you define a class that behaves like a collection,
provide members like `Add`, `Remove` and `Count` instead of `AddItem`, `Delete` or
`NumberOfItems`;

## avoid short names or names that can be mistaken for other names

`AV1712 Avoid short names or names that can be mistaken for other names`

**must**: although technically correct, following statements can be confusing;

```csharp
bool b001 = (lo == l0) ? (I1 == 11) : (lOl != 101);
```

## properly name properties

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

## name methods and local functions using verbs or verb-object pairs

`AV1720 Name methods and local functions using verbs or verb-object pairs`

**recommendation**: name a method or local function using a verb like `Show` or
a verb-object pair such as `ShowDialog`; a good name should give a hint on the what
of a member, and if possible, the why;

> don't include `And` in the name of a method or local function; that implies that
> it is doing more than one thing, which violates [the single responsibility principle](./class_design.md#single-responsibility-principle);

## name namespaces using names, layers, verbs and features

`AV1725 Name namespaces using names, layers, verbs and features`

**suggestion**: name namespaces using names, layers, verbs and features;

```csharp
// valid;
AvivaSolutions.Commerce.Web
NHibernate.Extensibility
Microsoft.ServiceModel.WebApi
Microsoft.VisualStudio.Debugging
FluentAssertion.Primitives
CaliburnMicro.Extensions
```

> never allow namespaces to contain the name of a type, but a noun in its plural
> form (e.g. `Collections`) is usually OK;

## use a verb or verb phrase to name an event

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

## use -ing and -ed to express pre-events and post-events

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

## prefix an event handler with `On`

`AV1738 Prefix an event handler with "On"`

**suggestion**: it is good practice to prefix the event handling method with `On`;

> example: a method that handles its own `Closing` event should be named
> `OnClosing` and a method that handles the `Click` event of its `okButton` field
> should be named `OkButtonOnClick`;

## use an underscore for irrelevant lambda parameters

`AV1739 Use an underscore for irrelevant lambda parameters`

**suggestion**: if you use a lambda expression (for instance, to subscribe to an
event) and the actual parameters of the event are irrelevant;

```csharp
button.Click += (_, __) => HandleClick();
```

## group extension methods in a class suffixed with `Extensions`

`AV1745 Group extension methods in a class suffixed with Extensions`

**suggestion**: if the name of an extension method conflicts with another member
or extension method, you must prefix the call with the class name; having them in
a dedicated class with the `Extensions` suffix improves readability;

## postfix asynchronous methods with `Async` or `TaskAsync`

`AV1755 Postfix asynchronous methods with Async or TaskAsync`

**recommended**: the general convention for methods and local functions that return
`Task` or `Task<TResult>` is to postfix them with `Async`; but if such a method
already exists, use `TaskAsync` instead;
