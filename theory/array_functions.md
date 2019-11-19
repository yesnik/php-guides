# PHP array functions

## array_filter()

```php
$a = [5, 0, [], 2, null, ''];
print_r( array_filter($a) );
/* Array
(
    [0] => 5
    [3] => 2
)
*/
```

## array_merge()

```php
$a = [1, 2];
$b = [2, 3];
$res = array_merge($a, $b);

print_r($res);
/*
Array
(
    [0] => 1
    [1] => 2
    [2] => 2
    [3] => 3
)
*/
```

## array_reduce()

```php
$a = [100, 2, 10];
$res = array_reduce($a, function($total, $item) {
    if (is_null($total)) {
        return $item;
    }
    return $total / $item;
}, null);

echo $res; // 5
```

## extract()

This method allows you to initialize variables from associative array.

```php
$vars = ['a' => 1, 'b' => 2];
extract($vars);

echo $a; // 1
echo $b; // 2
```
