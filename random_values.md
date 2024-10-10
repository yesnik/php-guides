# Random Values

In PHP we can generate random hash values:

```php
echo uniqid('abc_'); // 'abc_670778a9d1c22'

echo bin2hex(random_bytes(10)); // 'e3df56a704e33a2f89e8'

echo md5(openssl_random_pseudo_bytes(50) . uniqid('', true)); // '499635025d82b067cae412f45c47edbe' - 32 symbols
```
