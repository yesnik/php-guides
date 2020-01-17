# Mockery

[Mockery](https://github.com/mockery/mockery) is a simple flexible PHP mock object framework for use in unit testing.

## Mock class methods

```php
use PHPUnit\Framework\TestCase;

use App\TemperatureService;
use App\WeatherMonitor;

class WeatherMonitorTest extends TestCase
{
    public function tearDown(): void
    {
        Mockery::close();
    }

    /** @test */
    public function correct_average_is_returned_with_mockery()
    {
        $service = Mockery::mock(TemperatureService::class);

        $service->shouldReceive('getTemperature')->once()
            ->with('12:00')->andReturn(20);
        $service->shouldReceive('getTemperature')->once()
            ->with('14:00')->andReturn(26);

        $weather = new WeatherMonitor($service);

        $this->assertEquals(23, $weather->getAverageTemperature('12:00', '14:00'));
    }
}
```

## Mock static methods

Suppose that we have a class:

```php
class Manager
{
    public $email;

    public function __construct($email)
    {
        $this->email = $email;
    }

    public function notify(string $message)
    {
        return Notifier::send($this->email, $message);
    }
}
```

Test for `notify` method in this class:

```php
class ManagerTest extends TestCase
{
    public function tearDown(): void
    {
        Mockery::close();
    }

    /** @test */
    public function notify_returns_true()
    {
        $manager = new Manager('joe@mail.ru');

        $mock = Mockery::mock('alias:' . Notifier::class);
        $mock->shouldReceive('send')
            ->once()
            ->with($manager->email, 'hi')
            ->andReturn(true);

        $this->assertTrue($manager->notify('hi'));
    }
}
```
