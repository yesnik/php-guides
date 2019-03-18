# PHP show errors

Add these lines at the beginning of PHP file:

```php
error_reporting(E_ALL);
ini_set('display_errors', 1);
```

Read more about `display_errors` in [documentation](http://php.net/manual/en/errorfunc.configuration.php#ini.display-errors)
