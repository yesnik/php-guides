# PHPUnit methods

## Assertions

```php
$this->assertCount(3, [1, 2, 3]);
$this->assertEquals($var, 'someValue');
$this->assertInstanceOf(IteratorAggregate::class, $collection);
$this->assertTrue($var);
$this->assertFalse($var);
$this->assertArrayHasKey('some_key', $array);
```

## Useful methods

### setUp()

```php
class UserTest extends TestCase
{
    protected $user;

    public function setUp(): void
    {
        $this->user = new User();
    }
}
```
