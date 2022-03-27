# PHP 8.0 Features

## Constructor Property Promotion

```php
class Point {
    public function __construct(
        private float $x,
        private float $y
    ) {}
}

$a = new Point(1.1, 2.2);
var_dump($a);
```

## Union types

```php
declare(strict_types=1);

function getGreeting(int|string $name): string
{
    if (is_numeric($name)) {
        return 'Hello, Robot #' . $name;
    }
    return 'Welcome, ' . $name;
}

echo getGreeting(1); // Hello, Robot #1
echo getGreeting('Kenny'); // Welcome, Kenny
echo getGreeting(3.14); // Fatal error Uncaught TypeError: getGreeting(): 
                        // Argument #1 ($name) must be of type string|int, float given
```

Example from symfony/http-foundation [HeaderBag.php](https://github.com/symfony/http-foundation/blob/v6.0.6/HeaderBag.php#L130):

```php
    /**
     * Sets a header by name.
     *
     * @param string|string[]|null $values  The value or an array of values
     * @param bool                 $replace Whether to replace the actual value or not (true by default)
     */
    public function set(string $key, string|array|null $values, bool $replace = true)
    { 
    	// ...
```

## Weak map

A `WeakMap` class to create objects to be used as weak map keys that can be destroyed and removed from the weak map if there aren’t any further references to the key object.

In long-running processes, this would prevent memory leaks and improve performance.

```php
declare(strict_types=1);

$map = new WeakMap;
$obj = new stdClass;
$map[$obj] = 42;
var_dump($map);
unset($obj);
var_dump($map);
/*
object(WeakMap)#1 (0) {
}
*/
```

## Trailing Comma in Parameter List

```php
declare(strict_types=1);

function add(
    int $a,
    int $b, // trailing comma
) {
    return $a + $b;
}

echo add(2, 3); // 5
```

## Attributes v2

```php
declare(strict_types=1);

#[SomeAttribute('Hello world', 16)]
class Foo {}

#[Attribute]
class SomeAttribute {
    private string $message;
    private int $answer;
    public function __construct(string $message, int $answer) {
        $this->message = $message;
        $this->answer = $answer;
    }
}

$reflector = new \ReflectionClass(Foo::class);
$attrs = $reflector->getAttributes();

foreach ($attrs as $attribute) {
    var_dump($attribute->getName()); // "exampleAttribute"
    var_dump($attribute->getArguments()); // ["Hello world", 16]
    var_dump($attribute->newInstance());
    /*
    object(SomeAttribute)#3 (2) {
      ["message":"SomeAttribute":private]=>
        string(11) "Hello world"
      ["answer":"SomeAttribute":private]=>
        int(16)
    }
    */
}
```

## Named arguments

```php
function sub(int $first, int $second) {
    return $first - $second;
}

echo sub(second: 5, first: 8); // 3
```

## Nullsafe operator

When the left hand side of the operator evaluates to null the execution of the entire chain will stop and evalute to null. 
When it is not null it will behave exactly like the normal `->` operator. 

```php
$a = null;
var_dump($a?->add()?->sub()); // null

$a = 1;
var_dump($a?->add()); // Uncaught Error: Call to a member function add() on int
```

## Match

```php
echo match (1) {
	0 => 'a',
	1 => 'b',
	2 => 'c',
}; // 'b'

echo match (11) {
	0 => 'a',
	1, 11, 111 => 'b',
	2 => 'c',
}; // 'b'

echo match (11) {
	0 => 'a',
	1 => 'b',
}; // Uncaught UnhandledMatchError: Unhandled match value of type int 
```

## str_starts_with, str_ends_with, get_debug_type

```php
var_dump(str_starts_with('Laravel', 'Lara')); // true

var_dump(str_ends_with('Laravel', 'vel')); // true

var_dump(get_debug_type(new stdClass())); // stdClass
```
