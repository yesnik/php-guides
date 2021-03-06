# Magic methods in PHP

See [documentation](https://www.php.net/manual/en/language.oop5.magic.php)

PHP class has methods that will be called if attribute is not defined or not public.

## __toString

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
