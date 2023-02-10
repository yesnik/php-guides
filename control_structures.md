# Control Structures

See [docs](https://www.php.net/manual/en/language.control-structures.php)

### match

The match expression is similar to a switch statement but has some key differences:

- A `match` arm compares values strictly (`===`) instead of loosely as the `switch` statement does.
- A `match` expression returns a value.
- `match` arms do not fall-through to later cases the way `switch` statements do.
- A `match` expression must be exhaustive.

```php
$food = 'apple';

echo match ($food) {
    'apple', 'peach' => 'Fruit',
    'potato' => 'Vegetable',
}; // 'Fruit'

$food = 'cake';
echo match ($food) {
    'apple', 'peach' => 'Fruit',
    'potato' => 'Vegetable',
    default => 'Unknown',
}; // 'Unknown'

$food = 'cake';
echo match ($food) {
    'apple', 'peach' => 'Fruit',
    'potato' => 'Vegetable',
}; // Fatal error: Uncaught UnhandledMatchError: Unhandled match case 'cake'
```

**Handle non identity checks**

It is possible to use a `match` expression to handle non-identity conditional cases by using `true` as the subject expression.

```php
$age = 28;

$result = match (true) {
    $age >= 65 => 'senior',
    $age >= 25 => 'adult',
    $age >= 18 => 'young adult',
    default => 'kid',
};

var_dump($result); // 'adult'
```
