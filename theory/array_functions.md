# PHP array functions

## array_column()

[array_column](https://www.php.net/manual/en/function.array-column.php) — Return values from a single column in the input array

```php
$records = [
    [
        'id' => 2135,
        'first_name' => 'Kenny',
        'last_name' => 'Doe',
    ],
    [
        'id' => 3245,
        'first_name' => 'Jenny',
        'last_name' => 'Smith',
    ],
];

$first_names = array_column($records, 'first_name');
print_r($first_names);
/* 
Array
(
    [0] => Kenny
    [1] => Jenny
)
*/
```

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
```php
$filteredOperations = array_filter($operations, function($operation) {
    return $operation instanceof OperationInterface;
});
```
```php
$arr = [-1, 3, -3, 5];
$res = array_filter($arr, fn($n) => $n > 0);
print_r($res);
/* Array
(
    [1] => 3
    [3] => 5
) */
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

## array_reverse()

```php
array_reverse([1, 2]); // [2, 1]
```

## array_search()

Searches the array for a given value and returns the first corresponding key if successful

```php
$arr = [1, 3, 5, 3];
echo array_search(3, $arr); // 1
```

## array_sum()

```php
echo array_sum([1, 2, 3]); // 6
```

## asort() / arsort()

[asort](https://www.php.net/manual/en/function.asort) is used mainly when sorting associative arrays where the actual element order is significant.

- `asort(array &$array, int $flags = SORT_REGULAR): true` - sort an array in *ascending* order and maintain index association.
- `arsort()` - ... *descending* order

### Example 1

```php
$arr = [
    'a' => 10,
    'b' => 2,
    'c' => 3,
];

asort($arr);

print_r($arr);
/*
Array (
    [b] => 2
    [c] => 3
    [a] => 10
)
*/
```

### Example 2

```php
$arr = [
	'0' => 'a1',
	'1' => 'A10',
	'2' => 'a12',
	'3' => 'A2',
	'4' => 'A3',
];
asort($arr, SORT_NATURAL|SORT_STRING|SORT_FLAG_CASE);
print_r($arr);
/*
Array (
    [0] => a1
    [3] => A2
    [4] => A3
    [1] => A10
    [2] => a12
)
*/
```

## natsort()

```php
$array = ['car1.jpg', 'car2.jpg', 'car13.jpg'];

natsort($array);
// Natural order sorting
print_r($array);
/*
Array
(
    [0] => car1.jpg
    [1] => car2.jpg
    [2] => car13.jpg
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
