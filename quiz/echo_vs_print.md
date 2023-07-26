# `echo` vs `print`

### `echo` accepts many args, `print` - only one
```php
echo 1, 'hi'; // 1hi
print(1, 'hi'); // Parse error: syntax error, unexpected token ","
```

### `echo` doesn't return value, `print` returns `1`

```php
$a = echo 1; // Parse error: syntax error, unexpected token "echo"

$a = print(1); // Output: 1
var_dump($a); // int(1)
```

### `echo`, `print` works with or without parentheses `()`

```php
echo 1;
echo(1);

print 2;
print(2);
```

### Code example

```php
echo print null; // ?
```
<details><summary>Answer</summary>
1
</details>
