# Exceptions

## SPL Exceptions

See [documentation](https://secure.php.net/manual/en/spl.exceptions.php)

### InvalidArgumentException

Exception thrown if an argument is not of the expected type. 

```php
function set($key, $value)
{
    if (!$value instanceof UploadedFile) {
        throw new \InvalidArgumentException('An uploaded file must be an instance of UploadedFile.');
    }
}

set('a', 1); // Fatal error: Uncaught InvalidArgumentException: An uploaded file must be an instance of UploadedFile.
```
