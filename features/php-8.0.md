# PHP 8.0 Features

## Union types

```php
declare(strict_types=1);

function getGreeting(int|string $name): string
{
    if (is_numeric($name)) {
        return 'Hello, Robot #' . $name;
    }
    return 'Welcome, ' . $name;
}

echo getGreeting(1); // Hello, Robot #1
echo getGreeting('Kenny'); // Welcome, Kenny
echo getGreeting(3.14); // Fatal error Uncaught TypeError: getGreeting(): 
                        // Argument #1 ($name) must be of type string|int, float given
```
