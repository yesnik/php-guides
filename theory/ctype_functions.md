# Character type checking functions

## ctype_digit

This method returns `true` if all of the characters in the provided string are numerical.

```php
var_dump( ctype_digit('23') );   // true
var_dump( ctype_digit('23.1') ); // false
var_dump( ctype_digit('23,1') ); // false
var_dump( ctype_digit('10m') );  // false

var_dump( ctype_digit(55) );     // true, because chr(55) == '7'
var_dump( ctype_digit(65) );     // fales, because chr(65) == 'A'
```
