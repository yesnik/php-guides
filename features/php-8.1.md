# PHP 8.1 Features

## Enums

```php
enum Status {
  case Enabled;
  case Disabled;
}

class Cat {
    private Status $status;
    
    public function setStatus(Status $status)
    {
        $this->status = $status;
    }
    
    public function getStatus()
    {
        return $this->status;
    }
}
$tom = new Cat();
$tom->setStatus(Status::Enabled);
var_dump($tom->getStatus()); // enum(Status::Enabled)
```
```php
enum Status: int {
  case Enabled = 1;
  case Disabled = 0;
}

var_dump(Status::Enabled); // enum(Status::Enabled)
```

## Array unpacking with string keys

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
        public readonly DateTimeImmutable $bitrhdate,
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

The `never` type can be used to indicated that a function will actually stop the program flow. 
This can be done either by throwing an exception, calling exit or other similar functions.

```php
function dd(mixed $var): never
{
    var_dump($var);
    exit;
}

dd(1);
```
