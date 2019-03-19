# About PHP functions

## Splat operator (`...`)

```php
function add($a, $b) {
    return $a + $b;
}

$digits = [1, 2];

echo add(...$digits); // 3
```
