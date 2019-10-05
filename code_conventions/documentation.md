# documentation

## write comments and documentation in american english

`AV2301 Write comments and documentation in US English`

**must**: write comments and documentation in american english;

## document all `public`, `protected` and `internal` types and members

`AV2305 Document all public, protected and internal types and members`

**recommendation**: documenting code allows ide to pop-up the documentation when
your class is used somewhere else; furthermore, by properly documenting your
classes, tools can generate professionally looking class documentation;

## write xml documentation with other developers in mind

`AV2306 Write XML documentation with other developers in mind`

**recommendation**: write the documentation of your type with other developers in
mind; assume they will not have access to the source code and try to explain how
to get the most out of the functionality of your type;

## write msdn-style documentation

`AV2307 Write MSDN-style documentation`

**suggestion**: following the [msdn online](https://msdn.microsoft.com/en-US/)
help style and word choice helps developers find their way through your
documentation more easily;

## avoid inline comments

`AV2310 Avoid inline comments`

**recommendation**: if you feel the need to explain a block of code using a
comment, consider replacing that block with a method with a clear name;

## only write comments to explain complex algorithms or decisions

`AV2316 Only write comments to explain complex algorithms or decisions`

**must**: focus comments on the why and what of a code block and not the how;
avoid explaining the statements in words, but instead help the reader understand
why you chose a certain solution or algorithm and what you are trying to achieve;
if applicable, also mention that you chose an alternative solution because you ran
into a problem with the obvious solution;

## use comments for tracking work to be done later with caution

`AV2318 Don't use comments for tracking work to be done later`

**recommendation**: use comments for tracking work to be done later with caution;
try to fix issues as soon as possible, or delete them, opening new issue on
git repository; avoid such comments on `master` brunch and in release app version;
