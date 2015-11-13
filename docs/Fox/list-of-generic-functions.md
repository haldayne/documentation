The following table lists all the currently implemented generic functions:

| Class | Description |
|-------|-------------|
| [`CaptureErrors`][CaptureErrors] | Calls a function, capturing all PHP errors raised during the execution of the function. Provides access to the queue of captured errors afterwards. |
| [`Expression`][Expression] | Converts a string containing a PHP expression into a callable. Useful when you want to build small functions without the overhead of PHP closure syntax. |
| [`Retry`][Retry] | Retries a function until the function succeeds or fails the retry strategy. The default retry strategy attempts 5 calls and exponentially increases the delay between each call. You can define your own retry strategy. |
| [`Y`][Y] | Implements the [Y-combinator][pack4], which is a way to express a recursive algorithm with neither recursion nor iteration. |

[CaptureErrors]: https://github.com/haldayne/fox/blob/master/src/CaptureErrors.php
[Expression]: https://github.com/haldayne/fox/blob/master/src/Expression.php
[Retry]: https://github.com/haldayne/fox/blob/master/src/Retry.php
[Y]: https://github.com/haldayne/fox/blob/master/src/Y.php
