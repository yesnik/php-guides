# Password hash

- [password_hash](https://www.php.net/manual/en/function.password-hash.php) - Creates a password hash
- [password_verify](https://www.php.net/manual/en/function.password-verify.php) - Verifies that a password matches a hash

## Example

```php
$password = '123';

$passwordHash = password_hash($password, PASSWORD_BCRYPT, [
    'cost' => 12,
]);

// We can store this in a database
echo $passwordHash;

$result = password_verify($password, $passwordHash);

var_dump($result); // true
```
