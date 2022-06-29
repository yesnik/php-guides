# Anonimous Class

```php
class Cat {
    public function mew() {
        return 'Mew';
    }
}

$model = new class extends Cat {
    public function speak()
    {
        return 'Hi';
    }
};

echo (new $model)->mew(); // Mew
echo (new $model)->speak(); // Hi
```
