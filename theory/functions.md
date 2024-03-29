# Functions in PHP

## get_defined_functions

Returns an array of all defined functions:

```php
print_r(get_defined_functions(true));
```

## Arrays are passed by value

```php
function addZ($arr)
{
    $arr['z'] = 'zz';
    return $arr;
}

$arr = [5 => 'Five'];

var_dump(addZ($arr)); // [5 => 'Five', 'z' => 'zz']
var_dump($arr); // ?
```

### Ampersand allows to pass array by reference

```php
function addZ(&$arr)
{
    $arr['z'] = 'zz';
    return $arr;
}

$arr = [5 => 'Five'];

var_dump(addZ($arr)); // [5 => 'Five', 'z' => 'zz']
var_dump($arr); // ?
```

## Objects are passed by reference

It's not completely true - [see docs](https://www.php.net/manual/en/language.oop5.references.php).

```php
class Cat
{
    public $age = 10;
}

$tom = new Cat();

function setAge($object, $age)
{
    $object->age = $age;
    $object = null;
}

setAge($tom, 20);

// Wow! $tom is not null
echo $tom->age . PHP_EOL; // 20
```

In this case we can assign `null` to object inside the function:

```php
// ...
// We added `&`
function setAge(&$object, $age)
{
    $object->age = $age;
    $object = null;
}

setAge($tom, 20);

// $tom is null
echo is_null($tom); // 1
```
