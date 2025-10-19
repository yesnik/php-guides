# Reflection

PHP is providing a rich Reflection API that makes life easy to reach any area of the OOP structures.

- `ReflectionClass`: reports information about a class
- `ReflectionMethod`: reports information about a method
- `ReflectionParameter`: retrieves information about function’s or method’s parameters
- `ReflectionFunction`: reports information about a function
- `ReflectionClassConstant`: reports information about a class constant


## ReflectionClass

See [documentation](http://php.net/manual/en/class.reflectionclass.php)

```php
class Robot {
    public function __construct(
        private ?string $nickname = null
    ) {}

    /**
     * @return string
     */
    public function getNickname()
    {
        return $this->nickname;
    }

    /**
     * @param string $nickname
     */
    public function setNickname($nickname)
    {
        $this->nickname = $nickname;
    }
}

class Cyborg extends Robot {
}

$reflectionClass = new ReflectionClass('Robot');

$reflectionClassChild = new ReflectionClass('Cyborg');
```

### isInstantiable()

```php
// Returns true if we can create instance
var_dump( $reflectionClass->isInstantiable() ); // bool(true)
```

### newInstance()

```php
// Create instance with arguments
$object = $reflectionClass->newInstance('Kenny');
var_dump($object->getNickname()); // Kenny
```

### newInstanceWithoutConstructor()

```php
// Create instance without arguments
$object = $reflectionClass->newInstanceWithoutConstructor();
$object->setNickname('Jenny');
var_dump($object->getNickname()); // Jenny
```

### newInstanceArgs()

Creates a new class instance from given arguments.

```php
$reflectionClass = new ReflectionClass('Robot');
var_dump( $reflectionClass->newInstanceArgs(['Kenny']) );
/*
object(Robot)#2 (1) {
  ["nickname"]=>
  string(5) "Kenny"
}
*/
```

### getName()

```php
// Returns name of the class
var_dump($reflectionClass->getName()); // "Robot"
```

### getDocComment()

```php
// Returns documentation of class
var_dump($reflectionClass->getDocComment()); // Returns: 
```
```php
/**
 * Class Robot
 */
```

### getParentClass()

```php
print_r($reflectionClassChild->getParentClass());
/* Returns:
ReflectionClass Object
(
    [name] => Robot
)
*/
```

### getConstructor()

```php
var_dump ($reflectionClass->getConstructor());
/* Returns: 
object(ReflectionMethod)#3 (2) {
  ["name"]=>
  string(11) "__construct"
  ["class"]=>
  string(5) "Robot"
}
```

## ReflectionMethod

See [documentation](https://www.php.net/manual/en/class.reflectionmethod.php)

### getDocComment()

```php
$method = new ReflectionMethod('Robot', 'setNickname');

var_dump( $method->getDocComment() ); // Returns:
```
```php
/**
 * @param string $nickname
 */
```

### getParameters()

```php
$method = new ReflectionMethod('Robot', 'setNickname');
var_dump( $method->getParameters() ); // Returns:
/*
array(1) {
  [0]=>
  object(ReflectionParameter)#2 (1) {
    ["name"]=>
    string(8) "nickname"
  }
}
*/
```

### ReflectionMethod example

```php
// Here we create attribute that could be applied to method, and could be repeatable
#[Attribute(Attribute::TARGET_METHOD | Attribute::IS_REPEATABLE)]
class Given
{
    public function __construct(
        private string $title,    
    ) {}
    
    public function getTitle(): string
    {
        return $this->title;
    }
}

class Car
{
    // Apply attribute 'Given' to the method
    #[Given(title: 'product :arg1')]
    #[Given(title: 'product :arg2')]
    public function getModel(): string
    {
        return 'BMW';
    }
}


$reflectionMethod = new ReflectionMethod(Car::class, 'getModel');

// Get all attributes on the method
$methodAttributes = $reflectionMethod->getAttributes('Given');
$attribute = $methodAttributes[0];

echo "Attribute Name: " . $attribute->getName() . PHP_EOL; // 'Attribute Name: Given'
echo "Attribute Arguments: " . print_r($attribute->getArguments(), true) . PHP_EOL; // 'Attribute Arguments: Array( [title] => product :arg1 )'

// Instantiate the actual attribute class
$attributeInstance = $attribute->newInstance();
echo "Attribute Instance Param: " . $attributeInstance->getTitle() . PHP_EOL; // 'Attribute Instance Param: product :arg1'
```

## ReflectionParameter

See [documentation](https://www.php.net/manual/en/class.reflectionparameter.php)

