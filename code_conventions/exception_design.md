# exceptions design

## throw exceptions rather than returning some kind of status value

`AV1200 Throw exceptions rather than returning some kind of status value`

**recommendation**: a code base that uses return values to report success or
failure tends to have nested if-statements sprinkled all over the code; quite
often, a caller forgets to check the return value anyway; structured exception
handling has been introduced to allow you to throw exceptions and catch or
replace them at a higher layer; in most systems it is quite common to throw
exceptions whenever an unexpected situation occurs;

## provide a rich and meaningful exception message text

`AV1202 Provide a rich and meaningful exception message text`

**recommended**: the message should explain the cause of the exception,
and clearly describe what needs to be done to avoid the exception;

## throw the most specific exception that is appropriate

`AV1205 Throw the most specific exception that is appropriate`

**suggestion**: throw the most specific exception that is appropriate;

> example: if a method receives a `null` argument, it should throw
> `ArgumentNullException` instead of its base type `ArgumentException`;

## don't swallow errors by catching generic exceptions

`AV1210 Don't swallow errors by catching generic exceptions`

**must**: avoid swallowing errors by catching non-specific exceptions, such as
`Exception`, `SystemException` and so on, in application code; only in top-level
code, such as a last-chance exception handler, you should catch a non-specific
exception for logging purposes and a graceful shutdown of the application;

## properly handle exceptions in asynchronous code

`AV1215 Properly handle exceptions in asynchronous code`

**recommended**: when throwing or handling exceptions in code that uses
`async`/`await` or a `Task` remember the following two rules;

- exceptions that occur within an `async`/`await` block and inside a `Task`'s
  action are propagated to the awaiter;

- exceptions that occur in the code preceding the asynchronous block are
  propagated to the caller;
