# framework

## use c# type aliases instead of the types from the `System` namespace

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

## prefer language syntax over explicit calls to underlying implementations

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

## don't hard-code strings that change based on the deployment

`AV2207 Don't hard-code strings that change based on the deployment`

**suggestion**: don't hard-code strings that change based on the deployment;

> example: connection strings, server addresses, etc.; use `Resources`, the
> `ConnectionStrings` property of the `ConfigurationManager` class; maintain the
> actual values into the builtin or custom configuration;

## build with the highest warning level

`AV2210 Build with the highest warning level`

**must**: configure the development environment to use highest warning level for
the c# compiler, and enable the option treat warnings as errors; this allows the
compiler to enforce the highest possible code quality;

## avoid linq query syntax for simple expressions

`AV2220 Avoid LINQ query syntax for simple expressions`

**suggestion**: avoid linq query syntax for simple expressions;

```csharp
// invalid;
var query = from item in items where item.Length > 0 select item;

// valid;
var query = items.Where(item => item.Length > 0);
```

## use lambda expressions instead of anonymous methods

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

## only use the dynamic keyword when talking to a dynamic object

`AV2230 Only use the dynamic keyword when talking to a dynamic object`

**must**: the `dynamic` keyword has been introduced for working with dynamic
languages; using it introduces a serious performance bottleneck because the
compiler has to generate some complex reflection code;

use it only for calling methods or members of a dynamically created instance class
as an alternative to `Type.GetProperty()` and `Type.GetMethod()`;

## favor `async`/`await` over `Task` continuations

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
