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
