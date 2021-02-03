# Methods

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
