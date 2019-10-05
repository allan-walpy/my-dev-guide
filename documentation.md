# documentation and commenting

> [vs code extension](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)
> & [configuration](https://github.com/aaron-bond/better-comments#configuration)
>
> [visual studio extension](https://marketplace.visualstudio.com/items?itemName=OmarRwemi.BetterComments)

comments can be used for better reading of code;
but always put in mind, the best documentation and comments are code;

in following text will be used c# and bash language commentaries as example;

## important note

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

## todo comments

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

## fixme comments

**starts** with `//!TODO:FIXME:`, `#!TODO:FIXME:`

see [todo comments](#todo-comments);

## temporary solutions

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

## issue workaround

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

## code notice

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

## documentation comments

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

## any commented out code

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

## any other comments

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
