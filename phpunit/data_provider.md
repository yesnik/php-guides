# PHPUnit data provider

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
    public function testHello()
    {
        $world = new World();

        $this->assertEquals('Hello Kenny', $world->hello('Kenny'));
    }

    public function helloProvider()
    {
        return [
            ['Kenny', 'Hello Kenny'],
            [null, 'Hello stanger'],
        ];
    }
}
```
