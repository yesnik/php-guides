# Constants

### get_defined_constants()

Returns an associative array with the names of all the constants and their values.

```php
print_r(get_defined_constants(true));
/*
    [Core] => Array
        (
            [E_ERROR] => 1
            [E_WARNING] => 2
            ...
    [date] => Array
        (
            [DATE_ATOM] => Y-m-d\TH:i:sP
            [DATE_COOKIE] => l, d-M-Y H:i:s T
            ...
*/
```
