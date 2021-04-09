# PHP packages

## Images

- [intervention/image](https://github.com/Intervention/image) - PHP Image Manipulation. It has Laravel integration.

## DateTime

- [nesbot/carbon](https://github.com/briannesbitt/Carbon). It helps to return the date as a string: `5 minutes ago` or `1 month ago`.
```php
echo Carbon::now()->subMinutes(2)->diffForHumans(); // '2 minutes ago'
```

## Dependency Injection

- [PHP-DI](https://github.com/PHP-DI/PHP-DI)

## Security

- [roave/security-advisories](https://github.com/Roave/SecurityAdvisories). Package ensures that your application doesn't have installed dependencies with known security vulnerabilities.
