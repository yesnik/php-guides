# Reflection

PHP is providing a rich Reflection API that makes life easy to reach any area of the OOP structures.

- `ReflectionClass`: reports information about a class
- `ReflectionFunction`: reports information about a function
- `ReflectionParameter`: retrieves information about function’s or method’s parameters
- `ReflectionClassConstant`: reports information about a class constant
- `ReflectionMethod`: reports information about a method

## ReflectionClass

See [documentation](http://php.net/manual/en/class.reflectionclass.php)

```php
/**
 * Class Robot
 */
class Robot {
    public function __construct($nickname) {
        $this->nickname = $nickname;
    }
    
    /**
     * @return string
     */
    public function getRobotName(): string
    {
        return 'Robocop';
    }
}

class Robocop extends Robot {
}

$reflectionClass = new ReflectionClass('Robot');

$reflectionClassChild = new ReflectionClass('Robocop');
```

### isInstantiable()

```php
// Returns true if we can create instance
var_dump( $reflectionClass->isInstantiable() ); // bool(true)
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
$method = new ReflectionMethod('Robot', 'getRobotName');
var_dump( $method->getDocComment() ); // Returns:
```
```php
/**
 * @return string
 */
```
