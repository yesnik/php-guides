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
$sum = [1, 2] + [11, 22, 33]; // [1, 2, 33]
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
