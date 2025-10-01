# Token generator

```php
class TokenGenerator
{
    private const SYMBOLS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';

    public function getToken(int $length = 35): string
    {
        $length = strlen(self::SYMBOLS);

        $token = '';

        for ($i = 0; $i < $length; $i++) {
            $token .= self::SYMBOLS[random_int(0, $length - 1)];
        }

        return $token;
    }
}
```
