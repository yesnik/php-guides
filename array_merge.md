# array_merge($arr1, $arr2)

We can use php function `array_merge` to define default parameters.

```php
$default = [
    'a' => 11,
    'b' => 22
];

$new = array_merge(
    $default,
    [
        'b' => 2222,
        'c' => 33
    ]
);

var_dump($new);

/*
Returns:
array(3) {
  ["a"] => int(11)
  ["b"] => int(2222)
  ["c"] => int(33)
}
*/
```
