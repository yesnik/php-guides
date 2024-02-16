# Callbacks / Callables in PHP

[Callbacks](https://www.php.net/manual/en/language.types.callable.php) can be denoted by `callable` type hint as of PHP 5.4.

`callable` is a PHP data type. It means anything which can be called - closure, static/regular method, etc.

## Callback function examples

### 1. Simple callback

```php
function square(int $a): int {
    return $a ** 2;
}

echo call_user_func('square', 3); // 9
echo call_user_func('date', 'now'); // 220245
```

### 2. Static class method call

```php
class Calculator {
    public static function square($a) {
        return $a ** 2;
    }
}

echo call_user_func(['Calculator', 'square'], 3); // 9
echo ['Calculator', 'square'](3); // 9
```

### 3. Object method call

```php
$obj = new Calculator();
echo call_user_func([$obj, 'square'], 3); // 9
echo [$obj, 'square'](3); // 9

$obj = new DateTimeImmutable();
echo call_user_func([$obj, 'format'], 'Y-m-d'); // 2024-02-16
```

### 4. Static class method call

```php
echo call_user_func('Calculator::square', 3);
```

### 5. Objects implementing `__invoke` can be used as callables (since PHP 5.3)

```php
class Man {
    public function __invoke($name) {
        return 'Hello ' . $name;
    }
}

$man = new Man();
echo call_user_func($man, 'Kenny'); // 'Hello Kenny'
echo $man('Kenny'); // 'Hello Kenny'
```
