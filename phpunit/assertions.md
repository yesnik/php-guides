# Assertions

See [docs](https://phpunit.readthedocs.io/en/master/assertions.html)

```php
$this->assertCount(3, [1, 2, 3]);
$this->assertContains(2, [1, 2]);
$this->assertEmpty($var); // '', null, [], 0, false
$this->assertNotEmpty($var);
$this->assertEquals($var, 'someValue');
$this->assertInstanceOf(IteratorAggregate::class, $collection);
$this->assertSame(1, '1'); // will fail

$this->assertIsArray([1, 2]);
$this->assertIsInt(123);
$this->assertIsString('hi');
$this->assertIsNumeric('99');

$this->assertTrue($var);
$this->assertFalse($var);

$this->assertArrayHasKey('some_key', $array);
$this->assertStringStartsWith('wp_', 'wp_site'); // OK

$this->expectException(\App\Calculator\Exceptions\NoOperandsException::class);
$this->expectExceptionMessage('Queue is full');
```
