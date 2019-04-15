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

## ReflectionParameter

See [documentation](https://www.php.net/manual/en/class.reflectionparameter.php)

