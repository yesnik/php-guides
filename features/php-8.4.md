# PHP 8.4 Features

## Array functions

- `array_find()`
- `array_find_key()`
- `array_any()`
- `array_all()`

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

// Check, if any animal name is longer than 5 letters.
$result = array_any($array, function (string $value) {
    return strlen($value) > 5;
}); // true

// Check, if all animal names are shorter than 12 letters.
$result = array_all($array, function (string $value) {
    return strlen($value) < 12;
}); // true
```

## request_parse_body()

```php
// Parse request and store result in the $_POST and $_FILES superglobals.
[$_POST, $_FILES] = request_parse_body();

// Echo the content of some transferred file
echo file_get_contents($_FILES['file_name']['tmp_name']);
```
