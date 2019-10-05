# layout

## use a common layout

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

## order and group namespaces

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

## place members in a well-defined order

`AV2406 Place members in a well-defined order`

**must**: maintaining a common order allows other team members to find their way
in your code more easily; in general, a source file should be readable from top to
bottom, as if reading a book, to prevent readers from having to browse up and down
through the code file;

1. private fields and constants; *in a region*;
1. public constants;
1. public static read-only fields;
1. factory methods;
1. constructors and the finalizer;
1. events;
1. public properties;
1. other methods and private properties in calling order;

> declare local functions at the bottom of their containing method bodies after
> all executable code;

## be reluctant with `#region`

`AV2407 Be reluctant with #region`

**must**: be reluctant with `#region`; regions can be helpful, but can also hide
the main purpose of a class; therefore, use `#region` only for following;

- private fields and constants; *preferably in a private definitions region*;
- nested classes;
- interface implementations; *if the interface is not the main purpose of class*;

## use expression-bodied members appropriately

`AV2410 Use expression-bodied members appropriately`

**must**: favor expression-bodied member syntax over regular member syntax only if;

- the body consists of a single statement `and`;
- the body fits on a single line;
