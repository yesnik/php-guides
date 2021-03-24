# Mocks

### Mock without constructor args

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

If we don't provide stub for a method, this method will return `null` by default.

Under the [hood](https://github.com/sebastianbergmann/phpunit/blob/master/src/Framework/TestCase.php) `createMock` calls 
```php
$this->getMockBuilder($originalClassName)
    ->disableOriginalConstructor()
    // ...
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

**Stub only provided methods**

```php
// Example 1
$mailerMock = $this->getMockBuilder(Mailer::class)
                   ->disableOriginalConstructor()
                   ->setMethods(['send'])
                   ->getMock();

// Example 2
$handler = $this->getMockBuilder(DelayedPublicationHandler::class)
    ->setMethods(['getObjectForUpdate'])
    ->getMock();
$handler->method('getObjectForUpdate')->willReturn(true);
```

`setMethods(array $methods)` specifies the methods that are to be replaced with a configurable test double. 
The behavior of the *other methods is not changed*. If you call `setMethods(null)`, then no methods will be replaced.

**Method will throw exception**

```php
$user = new User;

$mailerMock = $this->createMock(Mailer::class);
$mailerMock->method('send')
    ->will($this->throwException(new Exception));

$user->setMailer($mailerMock);

$this->expectException(Exception::class);

$user->notify('hello');
```

**Method will return values from the map**

```php
/** @test */
public function correct_average_is_returned()
{
    $service = $this->createMock(TemperatureService::class);

    $map = [
        ['12:00', 20],
        ['14:00', 26],
    ];

    $service->expects($this->exactly(2))
            ->method('getTemperature')
            ->will($this->returnValueMap($map));

    $weather = new WeatherMonitor($service);

    $this->assertEquals(23, $weather->getAverageTemperature('12:00', '14:00'));
}
```

### Mock abstract class

Suppose we have abstract class:

```php
abstract class AbstractPerson
{
    protected $surname;

    public function __construct(string $surname)
    {
        $this->surname = $surname;
    }

    abstract protected function getTitle();

    public function getNameAndTitle()
    {
        return $this->getTitle() . ' ' . $this->surname;
    }
}
```

Test for this abstract class:

```php
/** @test */
public function name_and_title_is_returned_using_get_title()
{
    $mock = $this->getMockBuilder(AbstractPerson::class)
                 ->setConstructorArgs(['Green'])
                 ->getMockForAbstractClass();

    $mock->method('getTitle')
         ->willReturn('Dr.');          

    $this->assertEquals('Dr. Green', $mock->getNameAndTitle());
}
```

*Note:* Also you can install [Mockery](https://github.com/mockery/mockery) - simple and flexible PHP mock object framework. 
