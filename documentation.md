# Documentation and Commenting

> see also:
>
> - [extension on vs code](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)
>
> - [extension on visual studio](https://marketplace.visualstudio.com/items?itemName=OmarRwemi.BetterComments),
> *additional configuration required*
>
> - [defaults of vs code extension](https://github.com/aaron-bond/better-comments#configuration)

Default behavior for any comets is to delete them;
but there is some mechanics in workflow,
when commits can be handy

As different files/languages define different comments, some conditions will be used

The `{line_comment}` would used as inline comments
  `//` for `*.cs` files, `#!`for bash and yaml files;

The `{block_comment}` would be used for block comments
  `/*..*/` for `*.cs`, `#` at the begging every line in bash and yaml files;

## Important notice

> **Starts with** `{line_comment}!` and serves to mark some issues in code

```sharp
public int Length<T>(List<T> array)
{
    //! throws null exception;
    // TODO: add null check;
    return array.Count;
}
```

> Usually, such comments **deletes when they are no more relevant**

### Temporary solutions

> **Starts with** `{line_comment}!tmp;`;

One of the ways of using comment is mark temporary code solving some
issue, which must be replaced in the future

> NB! It is recommended to add them at the beginning and
> ending of modified code

```sharp
public int Length<T>(List<T> array)
{
    //!tmp;
    return (array ?? new List<T>()).Count;
    //!tmp;
}
```

> Usually, such comments must not be commented or pushed.
> Such comments **deletes always**.
>
> **Forbidden** in release code

#### Issue workaround

> **Example**
>
> ```markdown
> //!tmp issue #27;
> //issue https://gitlab.com/allan_walpy/dev_guide/issues/3
> [... any comments and lines of code ...]
> //!tmp issue end;
> ```
>
> - where `{issue}` is issue number on topic;

Sometime some issue can't be fixed at a time
(lower priority in iteration or they'll take too many time).
In such case code can have some so called

Very often can be added at the end of iteration, then `//!tmp` code can't be remove.

```sharp
public int Length<T>(List<T> array)
{
    //!tmp issue #4;
    //!issue https://github.com/vulmy/server/issues/4
    return (array ?? new List<T>()).Count;
    //!tmp issue end;
}
```

> Usually, **deletes when relevant issue resolved**

## Code notice

> **Starts with** `{inline_comment}?`

Explains non-obvious solutions in code.

```csharp
public static int Length<T>(this List<T> array)
{
    //? extension method can have source object null;
    //? see more at https://stackoverflow.com/a/847211 ;
    return array?.Count ?? 0;
}
```

> Usually, **deletes when no longer relevant**

## TODO comments

> **Starts with** `{inline_comment} TODO:`

In development of the Universe there were proto planets and proto galaxies.
In our development we have proto issues `TODO:` and proto bugs `TODO:FIXME`

```csharp
// TODO:FIXME: change method name to `GetLength()` to follow guidelines;
public static int Length<T>(this List<T> array)
{
    // TODO: make `IEnumerable<T>`, `IList<T>` versions;
    return array?.Count ?? 0;
}
```

> Usually, **deletes when resolved or issue on topic is open**

## FIXME comments

> **Starts with** `{inline_comment}FIXME:`

Avoid using such comment, and use `{inline_comment}TODO:FIXME` instead

> **Always replaced with `TODO:FIXME`**

## Documentation comments

> In C# starts with `///`;
>
> In bash or yaml starts with `##` or `#`;

In C# serves as code documentation which will be compiled in `*.xml`;

In bash or yaml may serve as file or code lines notation;

> **Deletes when no longer relevant**
> (for example, when method/class became private etc.)
