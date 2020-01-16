# PHPUnit test private methods

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

### Test

```php
use PHPUnit\Framework\TestCase;
use App\Item;

class ItemTest extends TestCase
{
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
}
```
