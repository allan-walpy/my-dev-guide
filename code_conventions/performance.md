# performance

## consider using `Any()` to determine whether an `IEnumerable<T>` is empty

`AV1800 Consider using Any() to determine whether an IEnumerable<T> is empty`

**suggestion**: when a member or local function returns an `IEnumerable<T>` or
other collection class that does not expose a `Count` property, use the `Any()`
extension method rather than `Count()` to determine whether the collection contains
items; if you do use `Count()`, you risk that iterating over the entire collection
might have a significant impact (such as when it really is an `IQueryable<T>` to
a persistent store);

> if you return an `IEnumerable<T>` to prevent changes from calling code consider
> using new read-only classes;

## only use async for low-intensive long-running activities

`AV1820 Only use async for low-intensive long-running activities`

**must**: the usage of `async` won't automatically run something on a worker
thread like `Task.Run` does; it just adds the necessary logic to allow releasing
the current thread, and marshal the result back on that same thread if a
long-running asynchronous operation has completed;

in other words, use `async` only for i/o bound operations;

## prefer `Task.Run` for cpu-intensive activities

`AV1825 Prefer Task.Run for CPU-intensive activities`

**must**: if you do need to execute a cpu bound operation, use `Task.Run` to
offload the work to a thread from the [thread pool](https://en.wikipedia.org/wiki/Thread_pool);
remember that you have to marshal the result back to your main thread manually;

## beware of mixing up `async`/`await` with `Task.Wait`

`AV1830 Beware of mixing up async/await with Task.Wait`

**must**: `await` does not block the current thread but simply instructs the
compiler to generate a state-machine; however, `Task.Wait` blocks the thread and
may even cause deadlocks;

## beware of `async`/`await` deadlocks in single-threaded environments

`AV1835 Beware of async/await deadlocks in single-threaded environments`

**must**: beware of `async`/`await` deadlocks in single-threaded environments;

```csharp
//invalid;

private async Task GetDataAsync()
{
    var result = await MyWebService.GetDataAsync();
    return result.ToString();
}

public ActionResult ActionAsync()
{
    var data = GetDataAsync().Result;

    return View(data);
}
```

`Result` property getter will block until the async operation has completed, but
since an `async` method could automatically marshal the result back to the
original thread (depending on the current `SynchronizationContext` or
`TaskScheduler`) and `asp.net` uses a single-threaded synchronization context,
they'll be waiting on each other;
