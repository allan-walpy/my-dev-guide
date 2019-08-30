# Documentation and Commenting

> see also:
>
> - [extension on vs code](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)
>
> - [extension on visual studio](https://marketplace.visualstudio.com/items?itemName=OmarRwemi.BetterComments),
> *additional configuration required*
>
> - [defaults of vs code extension](https://github.com/aaron-bond/better-comments#configuration)

Default behavior for any comets is to delete them; but there is some mechanics in workflow, when
commits can be handy

As different files/languages define different comments, some conditions will be used:

- `{line_coment}` would used as inline comments (`//` for `*.cs` files, `#!` for bash and yaml
  files);

- `{block_coment}` would be used for block comments (`/*..*/` for `*.cs`, `#` at the beggining
  every line in bash and yaml files)

## Important notice

> **Starts with** `{line_coment}!` and servs to mark some issues in code

```sharp
public int Length<T>(List<T> array)
{
    //! throws null exception;
    // TODO: add null check;
    return array.Count;
}
```

> Ussualy, such coments **deltes when they are no more relevant**

### Temporary solutios

> **Starts with** `{line_coment}!tmp;`;

One of the ways of using comment is mark temporary code solving some issue, which must be replaced
in the future

> NB! It is recomended to add them at the beggining and ending of modified code

```sharp
public int Length<T>(List<T> array)
{
    //!tmp;
    return (array ?? new List<T>()).Count;
    //!tmp;
}
```

> Usually, such comments must not been commeted or pushed. Such commets **deletes always**.
> **Forbidden** in result code of iteration (new version compilation)

#### Issue workaround

> **Template follows**
>
> ```markdown
> {inline_coment}!tmp issue #{issue};
> {inline_coment}issue https://gitlab.com/lausy-sudoku-game/core_dotnetcore/issues/{issue}
> [... any comments and lines of code ...]
> {inline_coment}!tmp issue end;
> ```
>
> - where `{issue}` is issue number on topic;

Sometime some issue can't be fixed at a time (lower priority in iteration or they'll take too many
time). In such case code can have some so called

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

> **Starts with** `{inline_coment}?`

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

In development of the Universe there were protoPlanets and protoGalaxies. In our development we
have protoIssues ` TODO:` and protoBugIssues ` TODO:FIXME`

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

Avoidusing such comment, and use `{inline_comment}TODO:FIXME` instead

> **Always replaced with `TODO:FIXME`**

## Documentation comments

> In C# starts with `///`;
>
> In bash or yaml starts with `##` or `#`;

In C# serves as code documentation which will be compilled in `*.xml`;

In bash or yaml may serve as file or code lines notation;

> **Deletes when no longer relevant** (for example, when method/class became private etc.)