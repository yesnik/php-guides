# Reflection

- `ReflectionClass`: reports information about a class
- `ReflectionFunction`: reports information about a function
- `ReflectionParameter`: retrieves information about function’s or method’s parameters
- `ReflectionClassConstant`: reports information about a class constant

## ReflectionClass

[Document](http://php.net/manual/en/class.reflectionclass.php)

```php
/**
 * Class Robot
 */
class Robot {
   /**
    * @return string
    */
   public function getRobotName(): string
   {
      return 'Robocop';
   }
}

$reflectionClass = new ReflectionClass('Robot');

// get name of the class
var_dump($reflectionClass->getName()); // "Robot"

// get documentation of class
var_dump($reflectionClass->getDocComment()); // Returns: 
// /**
//  * Class Robot
//  */

class Robocop extends Robot {
}

$class = new ReflectionClass('Robocop');

print_r($class->getParentClass());
/* Returns:
ReflectionClass Object
(
    [name] => Robot
)
*/
```

