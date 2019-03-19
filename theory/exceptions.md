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

### LogicException

Exception that represents error in the program logic. This kind of exception should lead directly to a fix in your code. 

```php
function deliver($amount)
{
    if ($amount > 10) {
        throw new \LogicException('We cannot deliver more than 10 items');
    }
}

deliver(15); // Fatal error: Uncaught LogicException: We cannot deliver more than 10 items
```
