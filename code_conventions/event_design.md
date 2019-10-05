# event design

## always check an event handler delegate for null

`AV1220 Always check an event handler delegate for null`

**must**: an event that has no subscribers is `null`; so before invoking, always
make sure that the delegate list represented by the event variable is not `null`;
invoke using the `null` conditional operator, because it additionally prevents
conflicting changes to the delegate list from concurrent threads;

```csharp
event EventHandler<NotifyEventArgs> Notify;

protected virtual void OnNotify(NotifyEventArgs args)
{
    Notify?.Invoke(this, args);
}
```

## use a protected virtual method to raise each event

`AV1225 Use a protected virtual method to raise each event`

**recommended**: complying with this guideline allows derived classes to handle
a base class event by overriding the protected method; the name of the protected
virtual method should be the same as the event name prefixed with `On`;

> example: the protected virtual method for an event named `TimeChanged` is named
> `OnTimeChanged`;
>
> derived classes that override the protected virtual method are not required to
> call the base class implementation; the base class must continue to work
> correctly even if its implementation is not called;

## consider providing property-changed events

`AV1230 Consider providing property-changed events`

**suggestion**: consider providing events that are raised when certain properties
are changed; such an event should be named `PropertyChanged`;

> if your class has many properties that require corresponding events, consider
> implementing the `INotifyPropertyChanged` interface instead. It is often used
> in the [presentation model](http://martinfowler.com/eaaDev/PresentationModel.html)
> and [model-view-viewModel](https://msdn.microsoft.com/en-us/magazine/dd419663.aspx)
> patterns;

## don't pass null as the sender argument when raising an event

`AV1235 Don't pass null as the sender argument when raising an event`

**must**: often an event handler is used to handle similar events from multiple
senders; the sender argument is then used to get to the source of the event;
always pass a reference to the source (typically `this`) when raising the event;
furthermore don't pass `null` as the event data parameter when raising an event;
if there is no event data, pass `EventArgs.Empty` instead of `null`;

> exception: on static events, the sender argument should be `null`;
