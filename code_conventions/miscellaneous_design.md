# miscellaneous design

## Use generic constraints if applicable

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

## evaluate the result of a linq expression before returning it

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

## do not use this and base prefixes unless it is required

`AV1251 Do not use this and base prefixes unless it is required`

**must**: in a class hierarchy, it is not necessary to know at which level a
member is declared to use it; refactoring derived classes is harder if that level
is fixed in the code;
