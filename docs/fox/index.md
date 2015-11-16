[`Haldayne\Fox`][1] provides algorithm strategies built using function objects.

Many algorithms are generic. That is, the way the algorithm functions is
independent of what the algorithm does to the underlying data. Let's take a
specific example: `array_sum` and `implode`. How do these work? They iterate
over an array, building up a return value. That is a [strategy][2] known as
[folding][3]. Is there another PHP function that folds? Yes, `array_reduce`
folds, and so we can build an equilvance between these:

| Specific function | Generic equivalent |
| ----------------- | ------------------ |
| `array_sum($a)` | `array_reduce($a, function ($t, $a) { return $t + $a; })` |
| `implode(',', $a)` | `array_reduce(array_slice($a, 1), function ($t, $a) { return $t . ',' . $a; }, $a[0])` |

You wouldn't use the generic equivalent in production code, because the
generic version is *less concise* than the specific function. But what if
the algorithm you need doesn't exist as a PHP native function? That's where
`Haldayne\Fox` comes in.

This package implements generic strategies in a concise way. Here are some
examples of strategies provided:

* Retry a piece of code until it works, failing after a certain number of
attempts and exponentially waiting between calls. You might use this to:
  * Copy a file over the network.
  * Push a message into a queue.
  * Connect to a remote service.
* Capture PHP errors triggered when running a piece of code. You might use
this to:
  * Run legacy code with errors turned off.
  * Capture run-time errors PHP emits.


[1]: https://github.com/haldayne/fox
[2]: http://c2.com/cgi/wiki?StrategyPattern
[3]: https://en.wikipedia.org/wiki/Fold_(higher-order_function)
