# PHPUnit methods

## Assertions

```php
$this->assertEquals($var, 'someValue');
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
