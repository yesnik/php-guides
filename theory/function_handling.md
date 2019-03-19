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

## func_get_arg

Returns an item from the argument list.

```php
function hi() {
    return func_get_arg(0) . ' ' . func_get_arg(1);
}
echo hi('Kenny'); // Kenny // Warning: func_get_arg(): Argument 1 not passed to function
echo hi('Joe', 'Doe'); // 'Joe Doe'
```
