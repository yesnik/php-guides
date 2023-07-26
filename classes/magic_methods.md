# Magic methods in PHP

[Magic methods](https://www.php.net/manual/en/language.oop5.magic.php) are special methods which override PHP's default's action 
when certain actions are performed on an object.

PHP has **17 magical methods**:

1. `__construct()`
2. `__destruct()`
3. `__call()`
4. `__callStatic()`
5. `__get()`
6. `__set()`
7. `__isset()`
8. `__unset()`
9. `__sleep()`
10. `__wakeup()`
11. `__serialize()`
12. `__unserialize()`
13. `__toString()`
14. `__invoke()`
15. `__set_state()`
16. `__clone()`
17. `__debugInfo()`

## 1. `__construct`

Classes which have a constructor method call this method on each newly-created object, 
so it is suitable for any initialization that the object may need before it is used.

### Call parent constructor - `parent::__construct()`

```php
class Vehicle
{
    public function __construct(
        private string $name
    ) {}
}

class Bus extends Vehicle
{
    private int $maxSpeed;

    public function __construct(string $name, int $maxSpeed) {
        parent::__construct($name);
        $this->maxSpeed = $maxSpeed;
    }
}
$schoolBus = new Bus('School Bus', 120);
var_dump($schoolBus);
/*
object(Bus)#1 (2) {
  ["name":"Vehicle":private]=>
  string(10) "School Bus"
  ["maxSpeed":"Bus":private]=>
  int(120)
}
*/
```

## 2. `__destruct`

The destructor method will be called as soon as there are no other references to a particular object, or in any order during the shutdown sequence.

```php
class Car
{
    public function __destruct()
    {
        echo 'Destruct';
    }
}
$bmw = new Car();
unset($bmw);
```

## 3. `__call(string $name, array $arguments): mixed`

It's triggered when invoking *inaccessible* methods in an object context.

```php
class Car
{
    public function __call(string $name, array $arguments)
    {
        echo 'Calling object method ' . $name . ', args: ' . 
        	print_r($arguments, true);
    }
    
    public function getName(): string
    {
    	return 'Some car';
    }
}
$bmw = new Car();
echo $bmw->getName(); // Some car
$bmw->someMethod(1, 'kenny');
/* Calling object method someMethod, args: Array
(
    [0] => 1
    [1] => kenny
) */
```

## 4. `public static __callStatic(string $name, array $arguments): mixed`

It's triggered when invoking *inaccessible* methods in a static context.

```php
class Car
{
    public static function __callStatic(string $name, array $arguments)
    {
        echo 'Calling object method ' . $name . ', args: ' . 
        	print_r($arguments, true);
    }
    
    public static function getName(): string
    {
    	return 'Some car';
    }
}

echo Car::getName(); // Some car
Car::someMethod(18, 'jenny');
/* Some carCalling object method someMethod, args: Array
(
    [0] => 18
    [1] => jenny
) */
```

## 5. `public __get(string $name): mixed`

It is utilized for reading data from *inaccessible* (protected or private) or non-existing properties.

```php
class Car
{
    public string $model = 'BMW';
    private string $color = 'red';
    
    public function __get(string $name)
    {
        echo 'Trying to get property: ' . $name;
    }
}

$bmw = new Car();
echo $bmw->model; // BMW
$bmw->year; // Trying to get property: year
$bmw->color; // Trying to get property: color
```

## 6. `public __set(string $name, mixed $value): void`

It is run when writing data to *inaccessible* (protected or private) or non-existing properties.

```php
class Car
{
    public string $model = 'BMW';
    private string $color = 'red';
    
    public function __set(string $name, mixed $value): void
    {
        echo 'Setting property: ' . $name . ' = ' . print_r($value, true);
    }
}

$bmw = new Car();
$bmw->model = 'Audi';
$bmw->color = 'blue'; // Setting property: color = blue
$bmw->someProperty = 'x'; // Setting property: someProperty = x
```
## 7. public __isset(string $name): bool

It is triggered by calling `isset()` or `empty()` on inaccessible (protected or private) or non-existing properties.

```php
class Car
{
    public string $model = 'BMW';
    private string $color = 'red';
    
    public function __isset(string $name): bool
    {
        echo 'Is set: ' . $name;
        return false;
    }
}

$bmw = new Car();
var_dump( isset($bmw->model) ); // true
var_dump( isset($bmw->color) ); // 'Is set: color' false
var_dump( isset($bmw->some) ); // 'Is set: some' false

var_dump( empty($bmw->model) ); // true
var_dump( empty($bmw->color) ); // 'Is set: color' false
var_dump( empty($bmw->some) ); // 'Is set: some' false
```

## 8. `public __unset(string $name): void`

It is invoked when `unset()` is used on *inaccessible* (protected or private) or non-existing *properties*.

```php
class Car
{
    public string $model = 'BMW';
    private string $color = 'red';
    
    public function __unset(string $name): void
    {
        echo 'Unset: ' . $name;
    }
}

$bmw = new Car();
unset($bmw->model);
unset($bmw->color); // Unset: color
unset($bmw->some); // Unset: some
```

## 9. `public __sleep(): array`

`serialize()` checks if the class has a function with the magic name `__sleep()`. 
If so, that function is executed prior to any serialization. 
It can clean up the object and is supposed to return an array with the names of all variables of that object that should be serialized.

The intended use of `__sleep()` is to commit pending data or perform similar cleanup tasks. 
Also, the function is useful if a very large object doesn't need to be saved completely.

## 10. `public __wakeup(): void`

Conversely, `unserialize()` checks for the presence of a function with the magic name `__wakeup()`. 
If present, this function can reconstruct any resources that the object may have.

The intended use of `__wakeup()` is to reestablish any database connections 
that may have been lost during serialization and perform other reinitialization tasks.

```php
class Connection
{
    protected $link;
    private $dsn, $username, $password;
    
    public function __construct($dsn, $username, $password)
    {
        $this->dsn = $dsn;
        $this->username = $username;
        $this->password = $password;
        $this->connect();
    }
    
    private function connect()
    {
        $this->link = new PDO($this->dsn, $this->username, $this->password);
    }
    
    public function __sleep()
    {
        return array('dsn', 'username', 'password');
    }
    
    public function __wakeup()
    {
        $this->connect();
    }
}
```
## 11. `public __serialize(): array`

`serialize()` checks if the class has a function with the magic name __serialize(). 
If so, that function is executed prior to any serialization. 
It must construct and return an associative array of key/value pairs that represent the serialized form of the object.

The intended use of `__serialize()` is to define a serialization-friendly arbitrary representation of the object.

**Note:** If both `__serialize()` and `__sleep()` are defined in the same object, only `__serialize()` will be called. 
  `__sleep()` will be ignored. If the object implements the `Serializable` interface, 
  the interface's `serialize()` method will be ignored and `__serialize()` used instead.

## 12. `public __unserialize(array $data): void`

Conversely, `unserialize()` checks for the presence of a function with the magic name `__unserialize()`. 
If present, this function will be passed the restored array that was returned from `__serialize()`. 
It may then restore the properties of the object from that array as appropriate.

**Note:** If both `__unserialize()` and `__wakeup()` are defined in the same object, 
    only `__unserialize()` will be called. `__wakeup()` will be ignored.

```php
class Connection
{
    protected $link;
    private $dsn, $username, $password;

    public function __construct($dsn, $username, $password)
    {
        $this->dsn = $dsn;
        $this->username = $username;
        $this->password = $password;
        $this->connect();
    }

    private function connect()
    {
        $this->link = new PDO($this->dsn, $this->username, $this->password);
    }

    public function __serialize(): array
    {
        return [
          'dsn' => $this->dsn,
          'user' => $this->username,
          'pass' => $this->password,
        ];
    }

    public function __unserialize(array $data): void
    {
        $this->dsn = $data['dsn'];
        $this->username = $data['user'];
        $this->password = $data['pass'];

        $this->connect();
    }
}
```

## 13. `__toString`

Method allows a class to decide how it will react when it is treated like a string. For example, what `echo $obj;` will print.

*Without __toString*

```php
class Cat {}

$tom = new Cat();
$tom->nickname = 'tom';

//echo $tom; // Recoverable fatal error: Object of class Cat could not be converted to string
```

*With __toString*

```php
class Dog
{
    public $nickname;
    
    public function __toString()
    {
        return 'A dog called ' . $this->nickname;
    }
}

$spike = new Dog();
$spike->nickname = 'Spike';

echo $spike; // A dog called Spike
```

## General example of magic methods

```php
<?php
class Cat {
    public $name;
    protected $nameSecret;
    
    public function getSecretName()
    {
        $this->nameSecret;
    }
    
    protected function think()
    {
        echo 'Thinking...';
    }
    
    public function __get($attr)
    {
        echo 'Attribute called: ' . $attr . PHP_EOL;
    }
    
    public function __set($attr, $val)
    {
        echo "Set attribute: {$attr}, val: {$val}" . PHP_EOL;
    }
    
    public function __isset($attr)
    {
        echo 'isset was called for attribute: ' . $attr . PHP_EOL;
    }
    
    public function __unset($attr)
    {
        echo 'unset was called for attribute: ' . $attr . PHP_EOL;
    }
    
    public function __call($attr, $args)
    {
        echo "Attribute {$attr} was called with args: " . print_r($args, true) . PHP_EOL;
    }
    
    public function __invoke($city, $age)
    {
        echo "Object was called as function - args: city: $city, age: $age";
    }
}

$tom = new Cat;
$tom->name;
$tom->nameSecret; // => Attribute called: nameSecret
$tom->aaa; // => Attribute called: age

$tom->name = 'Tom';
$tom->color = 'gray'; // Set attribute: color, val: gray

isset($tom->name);
isset($tom->nameSecret); // isset was called for attribute: nameSecret
isset($tom->aaa); // isset was called for attribute: aaa

unset($tom->name);
unset($tom->bbb); // unset was called for attribute: bbb

$tom->getSecretName();
$tom->think(); // Attribute think was called with args: Array()
$tom->jump('table', 'bed'); // Attribute jump was called with args: Array([0] => table [1] => bed)

$tom('NY', 15); // Object was called as function - args: city: NY, age: 15
```
