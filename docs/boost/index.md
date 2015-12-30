[`Haldayne\Boost`][1] improves the readability of PHP code that does a lot of
array manipulation.<sup>&dagger;</sup> Using a fluent interface that wraps any
collection-like data structure, you can build compact and readable expressions
to manipulate the elements in that collection.

`Map` is the Boost wrapper around collection-like data. That means, you can
create a map from any of these types:

* [`\Haldayne\Boost\Map`][2]
* [`\Haldayne\Boost\Contract\Arrayable`][3]
* [`\Haldayne\Boost\Contract\Jsonable`][4]
* [`\Traversable`][5]
* [`object`][6]
* [`array`][7]

The `Map` class gives you a fluent API on which you can solve many general
array-problems, like map/reduce:
```
$words = new Map(array ('bee', 'bear', 'beetle'));
$length = $words
    ->map(function ($word) { return strlen($word); })
    ->reduce(function ($total, $length) { return $total + $length; })
;
```

There are a boatload of built-in functions that apply to all Maps: [review the
API][8] for details. But there are also purpose-built classes that extend the
API to specific use-cases:

```
// join words together
$words = new MapOfStrings([ 'bleak', 'house' ]);
echo $words->join(' ');

// or sum all the odd integers between 1 and 10
// (Note use of short-hand code in grep statement)
$nums = new MapOfInts(range(0, 10));
echo $nums->all('1 == $_0 % 2')->sum();
```

These purpose-built classes demonstrate another feature of Boost's `Map`:
membership requirements. With these requirements, you can solve the ["type-hint
an array of Foo"][9] problem. In the example below, the `Bar` constructor
enforces that it takes only a collection of `Foo` instances:

```
class Foo { }

class Bar {
    public function __construct(MapOfFoo $foos) {
        $this->foos = $foos;
    }
}

class MapOfFoo extends \Haldayne\Boost\MapOfObjects {
    protected function allowed($value) { return $value instanceof Foo; }
}
```


<sup>&dagger;</sup> On the Boost roadmap are plans to also improve the
readability of heavy string manipulation code.

[1]: https://github.com/haldayne/boost
[2]: https://github.com/haldayne/boost/blob/master/src/Map.php
[3]: https://github.com/haldayne/boost/blob/master/src/Map.php
[4]: https://github.com/haldayne/boost/blob/master/src/Map.php
[5]: http://php.net/manual/en/class.traversable.php
[6]: http://php.net/manual/en/function.is-object.php
[7]: http://php.net/manual/en/function.is-array.php
[8]: http://haldayne.github.io/documentation/api/
[9]: http://stackoverflow.com/q/20763744/2908724
