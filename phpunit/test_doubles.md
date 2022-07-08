# Test Doubles

When we are writing a test in which we don't want to use a real depended-on component (DOC), we can replace it with a *Test Double*. 
The Test Double has to provide the same API as the real one so that the tested system thinks it is the real one.

PHPUnit has class [PHPUnit\Framework\TestCase](https://github.com/sebastianbergmann/phpunit/blob/main/src/Framework/TestCase.php) that provides methods: 

- `createStub()` - creates a *test stub* for the specified interface or clas
- `createMock()` - creates a *mock object* for the specified interface or class
- `createPartialMock()` - creates a partial mock object for the specified interface or class
- `getMockBuilder()` - returns a builder object to create mock objects using a fluent interface

They can be used in a test to automatically generate an object that can act as a test double for the specified original type (interface or class name).

## Stubs

**Stubbing** is the practice of replacing an object with a test double that optionally can return configured values.

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

```php
// Way 1
$greeting = self::createMock(Greeting::class);

// Way 2:
$greeting = self::createPartialMock(Greeting::class, ['getFullname']);

// Way 3:
$greeting = self::getMockBuilder(Greeting::class)
    ->setMethods(['getFullname'])
    ->getMock();

$greeting->method('getFullname')->willReturn('John Doe');

self::assertEquals('Dear John Doe', $greeting->getText());
```

**NOTE**: The mocked method **may not** be `private`, but it can be `protected`.

`setMethods(array $methods)` specifies the methods that are to be replaced with a configurable test double. 
The behavior of the *other methods is not changed*. If you call `setMethods(null)`, then no methods will be replaced.

In the `$greeting` mock object the `getFullname()` method would return `null` or you can override their return values. 
Any method within the class `Greeting` other than `getFullname()` will run their original code.

## Mocks objects

**Mocking** is the practice of replacing an object with a test double that *verifies expectations*, 
for instance asserting that a method has been called.

### Mock without constructor args

We can create mock objects to remove unnecessary dependencies.

```php
// Create mock object
$mailer = self::createMock(Mailer::class);

// Add expectation
$mailer
    ->expects(self::once())
    ->with(
        self::equalTo('hi@gmail.com'),
        self::equalTo('Hello')
    )
    ->method('send')
    ->willReturn(true);

self::assertTrue($mailer->send('hi@gmail.com', 'Hello'));
```

If we don't provide stub for a method, this method will return `null` by default.

Under the [hood](https://github.com/sebastianbergmann/phpunit/blob/main/src/Framework/TestCase.php) `createMock` calls 
```php
$this->getMockBuilder($originalClassName)
    ->disableOriginalConstructor()
    // ...
```

### Method will throw exception

```php
$user = new User();

$mailerMock = self::createMock(Mailer::class);
$mailerMock->method('send')
    ->will($this->throwException(new Exception));

$user->setMailer($mailerMock);

self::expectException(Exception::class);

$user->notify('hello');
```

### Method will return values from the map

```php
$service = self::createMock(TemperatureService::class);

$map = [
    ['12:00', 20],
    ['14:00', 26],
];

$service->expects(self::exactly(2))
        ->method('getTemperature')
        ->will(self::returnValueMap($map));

$weather = new WeatherMonitor($service);

self::assertEquals(23, $weather->getAverageTemperature('12:00', '14:00'));
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

## Links

- [Unit Testing Tutorial Part V: Mock Methods and Overriding Constructors](https://jtreminio.com/blog/unit-testing-tutorial-part-v-mock-methods-and-overriding-constructors/)
- [Managing return values with PHPUnit mock objects. Matt Zandstra](https://getinstance.com/return-values-phpunit-mock-objects/)
