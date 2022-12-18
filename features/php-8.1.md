# PHP 8.1 Features

## Enums

We can type-hint enums just like any other object:

```php
enum Status {
	case ENABLED;
	case DISABLED;
}

class Cat {
    public function __construct(
        public Status $status,
    ) {}
}
$tom = new Cat(Status::ENABLED);
var_dump($tom->status); // enum(Status::ENABLED)
var_dump($tom->status === Status::ENABLED); // bool(true)
```

### Backed enums

There's the possibility to assign string or integer values to enums:

```php
enum Status: int {
    case ENABLED = 1;
    case DISABLED = 0;
}

var_dump(Status::ENABLED); // enum(Status::ENABLED)
```
```php
enum Status: string {
    case PUBLISHED = 'published';
    case ARCHIVED = 'archived';
}
```

### Enum methods and interfaces

Enums can implement interfaces and define methods, just like normal classes.

```php
interface HasColor
{
    public function color(): string;
}

enum Status: int implements HasColor
{
    case ENABLED = 1;
    case DISABLED = 0;
    
    public function color(): string
    {
        return match($this) 
        {
            self::ENABLED => 'green',   
            self::DISABLED => 'gray',   
        };
    }
}

var_dump( Status::ENABLED->color() ); // "green"
```

### Serializing backed enums

Serializing them means you need a way to access the enum's value

```php
$value = Status::ENABLED->value; // 1
```

Restore an enum from a value:

```php
$status = Status::from(1); // Status::ENABLED
$status = Status::from(2); // Fatal error: Uncaught ValueError: 2 is not a valid backing value for enum "Status"
```

There's also a `tryFrom` that returns null if an unknown value is passed:

```php
$status = Status::tryFrom(1); // Status::ENABLED
$status = Status::tryFrom(2); // NULL
```

### Listing enum values

Get a list of all available cases within an enum:

```php
var_dump( Status::cases() );
/*
array(2) {
  [0]=>
  enum(Status::ENABLED)
  [1]=>
  enum(Status::DISABLED)
}
*/
```

## Array unpacking 

### With numeric keys

Array unpacking has been in PHP *since PHP 7.4*, though with a significant limitation: only arrays with *numeric keys* were allowed. 

```php
$arr = [2, 3];

$result = [1, ...$arr, 4]; // [1, 2, 3, 4]
```

If we try to unpack not numeric values, we'll get an error: *Fatal error: Cannot unpack array with string keys*.

### With string keys

```php
$a = ['a' => 1];
$b = ['b' => 2];
$arr = ['a' => 0, ...$a, ...$b, 'c' => 3];

print_r($arr);
/*
Array
(
    [a] => 1
    [b] => 2
    [c] => 3
)
*/
```

## `new` in initializers

```php
class Logger
{
    public function info(string $message)
    {
        echo '[INFO] ' . $message;
    }
}

class Process
{
    public function __construct(
        private Logger $logger = new Logger(), // We use 'new' here 
    ) {}
    
    public function run()
    {
        $this->logger->info('Run');
    }
}

$process = new Process();
$process->run();
```

## Readonly properties

```php
class Student {
    public function __construct(
        public readonly string $name,
        public readonly DateTimeImmutable $birthdate,
    ) {}
}

$kenny = new Student('Kenny', new DateTimeImmutable('1990-12-22'));
$kenny->name = 'Lenny'; // Uncaught Error: Cannot modify readonly property Student::$name 
```

## First-class callable syntax

You can now make a closure from a callable by calling that callable and passing it `...` as its argument:

```php
function add(int $a, int $b) { return $a + $b; }

$add = add(...);

echo $add(a: 1, b: 2); // 3
```

## Intersection types

Where union types require the input to be one of the given types, 
intersection types require the input to be all of the specified types. 
Intersection types are especially useful when you're working with lots of interfaces:

```php
interface HasId
{
    public function getId(): int;
}

interface HasTitle
{
    public function getTitle(): string;
}

class Post implements HasId, HasTitle
{
    public function __construct(
        private int $id,
        private string $title,
    ) {}
    public function getId(): int
    {
        return $this->id;
    }
    
    public function getTitle(): string
    {
        return $this->title;
    }
}

// Here we use Intersection Types
function getSlug(HasId&HasTitle $post)
{
    return $post->getId() . '-' . $post->getTitle();
}

$post = new Post(1, 'php');

echo getSlug($post); // 1-php
```

## Type `never`

The `never` type can be used to indicate that a function will actually stop the program flow. 
This can be done either by throwing an exception, calling exit or other similar functions.

```php
function dd(mixed $var): never
{
    var_dump($var);
    exit;
}

dd(1);
```
`never` differs from `void` in that `void` still allows the program to continue. 

## Function `array_is_list`

```php
$list = ["a", "b"];

var_dump(array_is_list($list)); // true

$notAList = [0 => "a", 3 => "b"];

var_dump(array_is_list($notAList)); // false
```

## Final class constants

Class constants in PHP can be overridden during inheritance. 
As of PHP 8.1, you can mark such constants as `final` in order to prevent this:

```php
class Animal
{
    final public const PLANET = "Earth";
}
 
class Cat extends Animal
{
    public const PLANET = "Mars";
    // Fatal error: Cat::PLANET cannot override final constant Animal::PLANET
}
```

## Function `fsync`

PHP 8.1 adds the `fsync` and `fdatasync` functions to force synchronization of file changes to disk 
and ensure operating system write buffers have been flushed before returning.

```php
$file = fopen("sample.txt", "w");

fwrite($file, "Some content");

if (fsync($file)) {
    echo "File has been successfully persisted to disk.";
}

fclose($file);
```
