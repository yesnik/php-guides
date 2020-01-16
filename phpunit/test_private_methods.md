# PHPUnit test private methods

It's possible to test private methods but *it's not recommended*, because private methods are the part of internal implementation of the class.

## Example

### Tested class

```php
class Item
{
    private function getToken()
    {
        return uniqid();
    }

    private function getPrefixedToken(string $prefix)
    {
        return uniqid($prefix);
    }
}
```

### Method without arguments

```php
/** @test */
public function getToken_returns_string()
{
    $item = new Item;

    $reflector = new ReflectionClass(Item::class);
    $method = $reflector->getMethod('getToken');
    $method->setAccessible(true);

    $result = $method->invoke($item);

    $this->assertIsString($result);
}
```

### Method with arguments

```php
/** @test */
public function prefixedToken_starts_with_prefix()
{
    $item = new Item;

    $reflector = new ReflectionClass(Item::class);
    $method = $reflector->getMethod('getPrefixedToken');
    $method->setAccessible(true);

    $result = $method->invokeArgs($item, ['wp_']);

    $this->assertStringStartsWith('wp_', $result);
}
```
