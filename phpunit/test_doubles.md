# Test Doubles

When we are writing a test in which we don't want to use a real depended-on component (DOC), we can replace it with a *Test Double*. 
The Test Double has to provide the same API as the real one so that the tested system thinks it is the real one.

PHPUnit has class [PHPUnit\Framework\TestCase](https://github.com/sebastianbergmann/phpunit/blob/master/src/Framework/TestCase.php) that provides methods: 

- `createStub()`, `createMock()` - they immediately return a test double object for the specified type
- `getMockBuilder()` - it allows to customize the test double generation 

They can be used in a test to automatically generate an object that can act as a test double for the specified original type (interface or class name).

## Stubs

**Stubbing** is the practice of replacing an object with a test double that (optionally) returns configured return values.

Tested class:

```php
class Greeting
{
    public function getText()
    {
        return "Dear {$this->getFullname()}"
    }
    // ...
}
```

### Stub methods of the class

Use `setMethods()` to define methods you want to stub:

```php
$greeting = $this->getMockBuilder(Greeting::class)
    ->setMethods(['getFullname'])
    ->getMock();

$greeting->method('getFullname')->willReturn('John Doe');

$this->assertEquals('Dear John Doe', $greeting->getText());
```

In the `$greeting` mock object the `getFullname()` method would return `null` or you can override their return values. 
Any method within the class `Greeting` other than `getFullname()` will run their original code.

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
