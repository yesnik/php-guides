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
$this->expectExceptionMessage('Queue is full');
```

## Mock

We can create mock objects to remove unnecessary dependencies.

```php
// Create mock object
$mailerMock = $this->createMock(Mailer::class);

// Stub method
$mailerMock
    ->expects($this->once())
    ->method('send')
    ->willReturn(true);

$this->assertTrue($mailer->send('hi@gmail.com', 'Hello'));
```

### Stub method

**Check value of arguments**

```php
$mailerMock = $this->createMock(Mailer::class);

$mailerMock
    ->expects($this->once())
    ->method('send')
    ->with(
        $this->equalTo('joe@gmail.com'),
        $this->equalTo('hello'),
    )
    ->willReturn(true);
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

### tearDown()

This method is useful to release unused resources after each test: opened files, sockets, big objects.

```php
class UserTest extends TestCase
{
    public function tearDown(): void
    {
        unset($this->user);
    }
}
```

### setUpBeforeClass(), tearDownAfterClass()

```php
class QueueTest extends TestCase
{
    public static function setUpBeforeClass(): void
    {
        // Connect to DB
    }

    public static function tearDownAfterClass(): void
    {
        // Disconnect from DB
    }
}
```
