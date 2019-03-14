# PHP Filesystem Functions

## dirname()

Returns absolute path of the parent's directory.

Script at `/home/nik/dirname.php`:

```php
echo __DIR__; // '/home/nik/temp'
echo dirname(__DIR__); // '/home/nik'
echo dirname(__DIR__, 2); // '/home'
```
