# PHPUnit methods

## Assertions

See [docs](https://phpunit.readthedocs.io/en/master/assertions.html)

```php
$this->assertCount(3, [1, 2, 3]);
$this->assertEquals($var, 'someValue');
$this->assertInstanceOf(IteratorAggregate::class, $collection);

$this->assertIsArray([1, 2]);
$this->assertIsInt(123);
$this->assertIsString('hi');
$this->assertIsNumeric('99');

$this->assertTrue($var);
$this->assertFalse($var);

$this->assertArrayHasKey('some_key', $array);

$this->expectException(\App\Calculator\Exceptions\NoOperandsException::class);
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
