# PHP Filesystem Functions

## basename

Returns trailing element of provided path.

```php
echo basename('/var/www/html/index.php'); // 'index.php'
echo basename('/var/www/html/'); // 'html'
var_dump( basename('/') ); // ''
```

## dirname

Returns absolute path of the parent's directory.

Script at `/home/nik/dirname.php`:

```php
echo __DIR__; // '/home/nik/temp'
echo dirname(__DIR__); // '/home/nik'
echo dirname(__DIR__, 2); // '/home'
```
