# Closure in PHP

The [Closure](https://www.php.net/manual/en/class.closure.php) class used to represent anonymous functions.

```php
function run ($fn) {
    var_dump($fn); // object(Closure)#1
    
    if ($fn instanceof Closure) {
        echo 'Closure here!';
    }
    
    return $fn();
}

$hello = function (){
    return 'hello';
};
echo run($hello);
```

## Closure::bindTo

[Closure::bindTo](https://www.php.net/manual/en/closure.bindto.php) duplicates the closure with a new bound object and class scope.

## Closure::bind

[Closure::bind](https://www.php.net/manual/en/closure.bind.php) is a static version of `Closure::bindTo()`.

```php
class Cat {
    protected $name;

    function __construct($name) {
        $this->name = $name;
    }

    function getClosure() {
        // returns closure bound to this object and scope
        return function() {
            return $this->name;
        };
    }
}

$tom = new Cat('Tom');
$closure = $tom->getClosure();
echo $closure(); // 'Tom'

$jack = new Cat('Jack');
$closure2 = $closure->bindTo($jack);
echo $closure2(); // 'Jack'

$ron = new Cat('Ron');
$closure3 = Closure::bind($closure, $ron);
echo $closure3(); // 'Ron'
```
