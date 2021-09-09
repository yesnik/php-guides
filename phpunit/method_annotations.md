# PHPUnit Method annotations 

## @test

```php
class UserTest extends TestCase
{
    /** @test */
    public function we_can_get_firstname()
    {
        $user = new User();
        $user->setFirstname('Kenny');

        $this->assertEquals($user->getFirstname(), 'Kenny');
    }
}
```

## @dataProvider

Arbitrary arguments are accepted by the test method. 
These arguments are provided by *data provider* method, 
which is a *public* and returns an array of arrays or objects. 

We can specify the data provider method by `@dataProvider` annotation.

```php
<?php

use PHPUnit\Framework\TestCase;
use App\Services\World;

class WorldTest extends TestCase
{
    /**
     * @dataProvider helloProvider
     */
    public function testHello($expected, $name)
    {
        $world = new World();

        $this->assertEquals($expected, $world->hello($name));
    }
    
    public function helloProvider(): array
    {
        return [
            ['Hello Kenny', 'Kenny'],
            ['Hello stanger', null],
        ];
    }
}
```

By default, each data provider is referenced by its array index.
In case of error PHPUnit could tell you, that the test failed with data set #0.

To prevent confusion when having lot of data providers, we can add title for each dataset:

```php
    public function helloProvider(): array
    {
        return [
            'Argument is added to greeting' => ['Hello Kenny', 'Kenny'],
            'Without argument stranger is used' => ['Hello stanger', null],
        ];
    }
```

## @depends

PHPUnit supports explicit dependencies between test methods. By using @depends annotations, we can ask PHPUnit to start one method after another.

```php
<?php

use PHPUnit\Framework\TestCase;

class DependTest extends TestCase
{
    public function testEmpty()
    {
        $value = [];
        $this->assertEmpty($value);
        return $value;
    }
    
    /**
     * @depends testEmpty
     */
    public function testPush(array $value)
    {
        array_push($value, 'first');
        $this->assertEquals('first', $value[count($value) - 1]);
        $this->assertNotEmpty($value);
        return $value;
    }
}
```

We declare one variable in `testEmpty()` and use the same variable in dependent methods. `testPush()` method depends on `testEmpty()` as the return value can be used in `testPush()` method. 

## @runInSeparateProcess

Indicates that a test should be run in a separate PHP process. 
By default, PHPUnit will attempt to preserve the global state from the parent process by serializing all globals 
in the parent process and unserializing them in the child process.

```php
/**
 * @runInSeparateProcess
 */
public function testSend() {}
```
