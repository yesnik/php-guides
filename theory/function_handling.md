# Function Handling

More info: [see docs](http://php.net/manual/en/book.funchand.php)

## func_num_args

Returns the number of arguments passed to the function.

```php
function hi() {
    return func_num_args();
}

echo hi(); // 0
echo hi('a', 11); // 2
echo hi(['a', 11]); // 1
```
