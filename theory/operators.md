# PHP operators

## The Execution Operator

PHP attempts to execute whatever is contained between the backticks as a command at the
server’s command line. The value of the expression is the output of the command.

```php
$out = `ls -la`;
echo '<pre>'.$out.'</pre>';
```

## Array operators

### Union operator `+`

```php
// Example 1
$a = [1, 2];
$b = [2, 3, 4];
$res = $a + $b;
print_r($res);
/*
Array
(
    [0] => 1
    [1] => 2
    [2] => 4
)
*/

// Example 2
$default = ['safe' => true, 'dbname' => 'app'];
$result = $default + ['safe' => false, 'user' => 'root'];

print_r($result);
/*
Array
(
    [safe] => 1
    [dbname] => app
    [user] => root
)
*/
```

### Equality operator `==`

```php
[1, 2] == ['1', 2]; // true
```

### Inequality operator `!=`, `<>`

```php
[1, 2] != ['1', 2]; // false
[1, 2] <> ['1', 2]; // false
```

### Identity operator `===`

```php
[1, 2] === ['1', 2]; // false
```

### Non-identity operator `!==`

```php
[1, 2] !== ['1', 2]; // true
```

## Type operator

```php
class Cat {};
$tom = new Cat;
var_dump($tom instanceof Cat); // true
```

## Splat operator (`...`)

```php
function add($a, $b) {
    return $a + $b;
}

$digits = [1, 2];

echo add(...$digits); // 3
```

