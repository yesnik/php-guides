# PHP array functions

## array_filter()

```php
// Example 1
$a = [5, 0, [], 2, null, ''];
print_r( array_filter($a) );
/* Array
(
    [0] => 5
    [3] => 2
)
*/

// Example 2
$filteredOperations = array_filter($operations, function($operation) {
    return $operation instanceof OperationInterface;
});
```

## array_map()

```php
$a = [1, 2, 3];
$result = array_map(function($x) {
    return $x ** 2;
}, $a);

print_r($result); // [1, 4, 9]
```

## array_merge()

### Flatten arrays

*Example 1*

```php
$a = [1, 2];
$b = [2, 3];
$res = array_merge($a, $b);

print_r($res); // [1, 2, 2, 3]
```

*Example 2*

```php
$array = [
    [
        'a' => 1,  
    ],
    [
        'b' => 2,
        'c' => 3,
    ],
];

$result = array_merge([], ...$array);

var_dump($result);
/*
array(3) {
  ["a"] => int(1)
  ["b"] => int(2)
  ["c"] => int(3)
}
*/
```

### Default values

We can use this function to define default parameters.

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

## array_rand()

Returns random *key* of array.

```php
$arr = ['a', 'b', 'c'];
echo $arr[ array_rand($arr) ]; // 'c'
echo array_rand($arr); // 1
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

## asort()

Sort an array in *ascending* order and maintain index association.

```php
$arr = [
    'a' => 10,
    'b' => 2,
    'c' => 3
];

asort($arr);

print_r($arr);
/*
Array
(
    [b] => 2
    [c] => 3
    [a] => 10
)
*/
```

## extract()

This method allows you to initialize variables from associative array.

```php
$vars = ['a' => 1, 'b' => 2];
extract($vars);

echo $a; // 1
echo $b; // 2
```
