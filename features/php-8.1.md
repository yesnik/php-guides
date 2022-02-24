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
