[`Haldayne\Fox`][1] provides implementation of generic, higher-order functions.
What does this mean?

Many algorithms are generic. That is, the way the algorithm functions is
independent of what the algorithm does to the underlying data. Let's take a
specific example: `array_sum` and `implode`.  How do these function?  They
iterate over an array, building up a return value.  Is there another PHP
function that does that?  Yes, `array_reduce` and we can build an equilvance
between them:

| Specific Function | Generic equivalent |
| ----------------- | ------------------ |
| `array_sum($a)` | `array_reduce($a, function ($t, $a) { return $t + $a; })` |
| `implode(',', $a)` | `array_reduce(array_slice($a, 1), function ($t, $a) { return $t . ',' . $a; }, $a[0])` |

You wouldn't do this in real life, but it demonstrates the concept of higher-
order functions: one function taking another so as to abstract common behavior
to one place.

As program designers, we want our language powerful enough to express the
*concept of summation*, not just provide functions to compute sums.  PHP is
that powerful, but we need the generic functions to take advantage of this
generic *programming.

Learn more:

* [Design and advantages of the `Haldayne\Fox` generic function][2]
* [List of generic functions][3]

[1]: https://github.com/haldayne/fox
[2]: http://haldayne-docs.readthedocs.org/en/latest/fox-design-and-advantage/
[3]: http://haldayne-docs.readthedocs.org/en/latest/fox-list-of-generic-functions/
