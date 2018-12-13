# PHP Data Types

PHP is called a weakly typed or dynamically typed language. In PHP, the *type* of a variable is determined by the value assigned to it.

PHP supports the following basic data types:

- **Integer**. Used for whole numbers. Functions: `is_long()`, `is_int()`, `is_integer()`
- **Float** (also called double). Used for real numbers. Functions: `is_double()`, `is_float()`, `is_real()`
- **String**. Used for strings of characters. Function: `is_string()`
- **Boolean**. Used for `true` or `false` values. Function: `is_bool()`
- **Array**. Used to store multiple data items. Function: `is_array()`
- **Object**. Used for storing instances of classes. Function: `is_object()`

Special types:

- **NULL**. Variables that have not been given a value, have been unset, or have been given the specific
value `NULL`. Function: `is_null()`
- **resource**. Certain built-in functions (such as database functions) return variables that have the type
resource. They represent external resources (such as database connections). Function: `is_resource()`
- **callable**. This is essentially a function that is passed to other function. Function: `is_callable()`
- **variable variable**. Variable variables enable you to change the name of a variable dynamically:

```
$hello = 'Hi';
$action = 'hello';
echo $$action; // Hi
```
