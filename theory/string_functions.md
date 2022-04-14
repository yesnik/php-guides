# PHP String functions

## htmlspecialchars

Converts special characters to HTML entities. It helps to avoid XSS issues.

```php
echo htmlspecialchars('<a href="/">home</a>'); // &lt;a href=&quot;/&quot;&gt;home&lt;/a&gt;
```

## str_repeat

```php
echo str_repeat('ab', 3); // 'ababab'
```

## str_starts_with

PHP 8

```php
var_dump( str_starts_with('Kate', 'K') ); // bool(true)
```

## trim

```php
trim(" hey  \n"); // 'hey'
trim('__hey_world_', '_'); // 'hey_world'
trim('_,_,_hey_world_x', '_,x'); // 'hey_world'
```

## uniqid

[uniqid](https://www.php.net/manual/en/function.uniqid.php) gennerates prefixed unique identifier based on the current time in microseconds.

```php
uniqid(); // 5e203c4c794d0
uniqid('server1_'); // server1_5e203c806552f
uniqid('', true); // 5e203ca619bc02.47789457
```
