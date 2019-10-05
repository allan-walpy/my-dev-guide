# member design

## allow properties to be set in any order

`AV1100 Allow properties to be set in any order`

**must**: properties should be stateless with respect to other properties;

there should not be a difference between first setting property `DataSource` and
then `DataMember` or vice-versa;

## use a method instead of a property

`AV1105 Use a method instead of a property`

**suggestion**: use a method instead of a property;

- if the work is more expensive than setting a field value;

- if it represents a conversion such as the `Object.ToString` method;

- if it returns a different result each time it is called, even if the arguments
  didn't change;

  > example: the NewGuid method returns a different value each time it is called;

- if the operation causes a side effect such as changing some internal state not
  directly related to the property

  > this violates the [command query separation principle](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation);

> exception: populating an internal cache or implementing lazy-loading;

## don't use mutually exclusive properties

`AV1110 Don't use mutually exclusive properties`

**must**: having properties that cannot be used at the same time typically
signals a type that represents two conflicting concepts; even though those
concepts may share some of their behavior and states, they obviously have
different rules that do not cooperate;

> this violation is often seen in domain models and introduces all kinds of
> conditional logic related to those conflicting rules, causing a ripple effect
> that significantly increases the maintenance burden;

## a property, method or local function should do only one thing

`AV1115 A property, method or local function should do only one thing`

**must**: a method body should have a single responsibility;

## don't expose stateful objects through static members

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

## return a builtin collection interface instead of a concrete collection class

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

## strings or collections should never be null

`AV1135 Properties, arguments and return values representing strings or
collections should never be null`

**must**: returning `null` can be unexpected by the caller; always return an
empty collection or an empty string instead of a `null` reference.

> when your member return `Task` or `Task<T>`, return `Task.CompletedTask` or
> `Task.FromResult()`; this also prevents cluttering your code base with
> additional checks for null, or even worse, `string.IsNullOrEmpty()`;

## define parameters as specific as possible

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

## consider using domain-specific value types rather than primitives

`AV1140 Consider using domain-specific value types rather than primitives`

**suggestions**: instead of using strings, integers and decimals for representing
domain-specific types such as an ISBN number, an email address or amount of money,
consider creating dedicated value objects that wrap both the data and the
validation rules that apply to it; by doing this, you prevent ending up having
multiple implementations of the same business rules, which both improves
maintainability and prevents bugs;
