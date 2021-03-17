# PHP Types

[Documentation](https://www.php.net/manual/en/language.types.intro.php) says that PHP supports **10** primitive types.

Memo: 4 4 2

### 4 scalar types

- int. Used for whole numbers. Functions: `is_long()`, `is_int()`, `is_integer()`
- float (also called double). Used for real numbers. Functions: `is_double()`, `is_float()`, `is_real()`
- string. Used for strings of characters. Function: `is_string()`
- bool. Used for `true` or `false` values. Function: `is_bool()`

### 4 compound types

- array. Used to store multiple data items. Function: `is_array()`
- object. Used for storing instances of classes. Function: `is_object()`
- [callable](https://www.php.net/manual/en/language.types.callable.php). 
  This is essentially a function that is passed to other function. Function: `is_callable()`
- [iterable](https://www.php.net/manual/en/language.types.iterable.php). Introduced in PHP 7.1. 
  It accepts any array or object implementing the `Traversable` interface. Both of these types are iterable using `foreach` and can be used with `yield` from within a generator.

### 2 special types

- resource. Certain built-in functions (such as database functions) return variables that have the type
resource. They represent external resources (such as database connections). Function: `is_resource()`
- NULL. Variables that have not been given a value, have been unset, or have been given the specific
value `NULL`. Function: `is_null()`
