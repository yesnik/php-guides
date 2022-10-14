# preg_match

Perform a regular expression match.

```php
var_dump( preg_match('/^[А-ЯЁ][а-яё]+$/u', 'Иванов') ); // 1
var_dump( preg_match('/^[А-ЯЁ][а-яё]+$/u', 'Ivanov') ); // 0
```
We can check string for Russian symbols:

```php
if (!preg_match('/^[А-ЯЁ][а-яё]+$/u', $value)) {
    throw InvalidArgumentException('Only Russian letters are allowed');
}
```

**Note:** Don't forget to place `u` flag (Unicode) at the end of RegExp.
