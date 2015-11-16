For the most part, all objects implement the [Callable Object Pattern][1].
The constructor for each class takes a function, and this function will be
called when the object is invoked. Here's an example:

```php
// call some code, capturing PHP triggered errors locally
use \Haldayne\Fox\CaptureErrors;
$capture = new CaptureErrors(function ($input) { trigger_error('Oi!'); return $input; });
$capture->setCapturedErrorTypes(E_ALL|~E_STRICT);
$result = $capture(false);
if (! $result) {
    die($capture->getCapturedErrors()->pop()->get('message'));
}
```

The generic algorithm here is "capture PHP errors". All `CaptureErrors` does
is wrap the function in a temporary error handler via [`set_error_handler`][2].
You could do this yourself, manually. But there are advantages to building
generic algorithms.

# And that advantage is?

Composability. Here is a larger example:

```php
use Haldayne\Fox\Retry, Haldayne\Fox\CapturingErrors;

$retry = new Retry(
    $capture = new CapturingErrors(
        function ($src, $dst) {
            return copy($src, $dst);
        }
    )
);

$capture->setCapturedErrorTypes(E_ALL|~E_STRICT);
$retry->setAttempts(5);

$result = $retry('ssh.sftp2://user@host/file', 'file');
if (false === $result) {
    die($capture->getCapturedErrors()->implode(PHP_EOL);
}
```

Notice how `Retry` consumes `CapturingErrors`. These are two independent
function objects, oblivious to how each other operates. But they're composed
together to solve a problem: retry copying a file while recording the emitted
PHP errors for diagnostic purposes.

Take a moment and think about how you would do this with pure functions. In
a reusable way. So that you could swap the order in which they're called.

To capture errors, you have to modify and restore global state. To retry, you
have to run a loop. You're going to need at least two functions to make these
reusable, then you're going to have to wire them up. It's going to be
complicated, perhaps impossible.

But with higher-order functions (functions that accept functions), it's
astonishlingly easy.

The PHP magic method __invoke() is the syntactic sugar that makes it possible
for these two functions to be completely indepedent. They know nothing of
one another, but rely on the defined interface: each is callable.

Next: [List of all generic functions][3]

[1]: http://cerebriform.blogspot.com/2015/11/wielding-php-magic-with-callable-object.html
[2]: http://php.net/manual/en/function.set-error-handler.php
[3]: list-of-generic-functions.md
