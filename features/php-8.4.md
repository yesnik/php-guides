# PHP 8.4 Features

## Array functions

### array_find(), array_find_key()

```php
$array = [
    'a' => 'dog',
    'e' => 'goose',
    'f' => 'elephant'
];

// Find the first animal with a name longer than 4 characters.
$result = array_find($array, function (string $value) {
    return strlen($value) > 4;
}); // 'goose'

// Find the first index of the animal with a name longer than 4 characters.
$result = array_find_key($array, function (string $value) {
    return strlen($value) > 4;
}); // 'e'
```
