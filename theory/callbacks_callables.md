# Callbacks / Callables in PHP

[Callbacks](https://www.php.net/manual/en/language.types.callable.php) can be denoted by `callable` type hint as of PHP 5.4.

## Callback function examples

```php
// An example callback function
function square($a) {
    return $a ** 2;
}

// An example callback method
class Calculator {
    static function square($a) {
        return $a ** 2;
    }
}

// Type 1: Simple callback
echo call_user_func('square', 3); // 9

// Type 2: Static class method call
echo call_user_func(['Calculator', 'square'], 3); // 9
echo ['Calculator', 'square'](3); // 9

// Type 3: Object method call
$obj = new Calculator();
echo call_user_func([$obj, 'square'], 3); // 9
echo [$obj, 'square'](3); // 9

// Type 4: Static class method call
echo call_user_func('Calculator::square', 3);

// Type 5: Objects implementing __invoke can be used as callables (since PHP 5.3)
class Man {
    public function __invoke($name) {
        return 'Hello ' . $name;
    }
}

$man = new Man();
echo call_user_func($man, 'Kenny'); // 'Hello Kenny'
echo $man('Kenny'); // 'Hello Kenny'
```
