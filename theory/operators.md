# PHP Operators

[Documentation](https://www.php.net/manual/en/language.operators.php) says about many operators.

An operator is something that takes one or more values (or expressions, in programming jargon) 
and yields another value (so that the construction itself becomes an expression).

Operators can be grouped according to the number of values they take: 

- Unary operators. They take only one value, for example ! (the logical not operator) or ++ (the increment operator). 
- Binary operators. They take two values, such as the familiar arithmetical operators + (plus) and - (minus), and the majority of PHP operators fall into this category.
- Ternary operator. It is `? :`, which takes three values.

## Arithmetic Operators

- `+$a` - Identity - Conversion of $a to int or float as appropriate.
    ```php
    $a = '12';
    var_dump(+$a); // (int) 12
    ```
- `$a % $b` - Modulo - Remainder of $a divided by $b. The result of the modulo operator % has the same sign as the dividend. 
    The result of $a % $b will have the same sign as $a.
    ```php
    echo 8 % 3; // 2
    echo -8 % 3; // -2
    ```
- `$a ** $b` - Exponentiation - Result of raising $a to the $b'th power.
    ```php
    echo 2 ** 3; // 8
    ```

## Assignment Operators

Assignment means that the left operand gets set to the value of the expression on the right.

```
$b = ($a = 2) + 3;
echo $a; // 2
echo $b; // 5
```

### Arithmetic Assignment Operators

- `$a *= $b` - `$a = $a * $b` - Multiplication
- `$a %= $b` - `$a = $a % $b` - Modulus
- `$a **= $b` - `$a = $a ** $b` - Exponentiation

### Other Assignment Operators

- `$a .= $b` - `$a = $a . $b` - String Concatenation
- `$a ??= $b` - `$a = $a ?? $b` - Null Coalesce

## Comparison Operators

- `$a == $b` - Equal - true if $a is equal to $b after type juggling.
- `$a === $b` - Identical - true if $a is equal to $b, and they are of the same type.
- `$a <=> $b` - Spaceship
    ```php
    echo 10 <=> 20; // - 1
    echo 20 <=> 10; // 1
    echo 10 <=> 10; // 0
    ```

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

