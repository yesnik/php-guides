# Assignment Operator

## Copy array by value

Array `$two` won't be modified:

```php
$one = [
	'a' => 1,
	'b' => 2,
];

$two = $one;

$one['a'] = 11;

print_r($one);
/* Array (
    [a] => 11
    [b] => 2
)
*/

print_r($two);
/* Array (
    [a] => 1
    [b] => 2
)
*/
```
