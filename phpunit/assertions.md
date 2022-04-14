# Assertions

See [docs](https://phpunit.readthedocs.io/en/master/assertions.html)

```php
self::assertCount(3, [1, 2, 3]);
self::assertContains('a', ['a', 'b']);
self::assertNotContains('c', ['a', 'b']);
self::assertEmpty($var); // '', null, [], 0, false
self::assertNotEmpty($var);
self::assertEquals($var, 'someValue');
self::assertInstanceOf(IteratorAggregate::class, $collection);
self::assertSame(1, '1'); // will fail

self::assertIsArray([1, 2]);
self::assertIsInt(123);
self::assertIsString('hi');
self::assertIsNumeric('99');

self::assertTrue($var);
self::assertFalse($var);

self::assertArrayHasKey('some_key', $array);
self::assertStringStartsWith('wp_', 'wp_site'); // OK

self::expectException(\App\Calculator\Exceptions\NoOperandsException::class);
self::expectExceptionMessage('Queue is full');
```
