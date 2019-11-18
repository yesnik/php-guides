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

## extract()

This method allows you to initialize variables from associative array.

```php
$vars = ['a' => 1, 'b' => 2];
extract($vars);

echo $a; // 1
echo $b; // 2
```
