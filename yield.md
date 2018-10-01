# yield in PHP

`yield` allows us to create generators:

```php
function genNumsTo($to) {
    for ($i = 1; $i <= $to; $i++) {
        yield $i;
    }
}

$generator = genNumsTo(3);

foreach ($generator as $num) {
    echo $num;
}

// Result: 123
```
