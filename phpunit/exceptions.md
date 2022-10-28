# Exception tests

## Example 1. Method expectException()

To perform assertion after exception has been thrown:

```php
$this->expectException(Exception::class);

try {
    $service->send($message);
} catch (Exception $exception) {
    // Assert that `$service->status` was changed
    $this->assertEquals('error', $service->status);
    
    // Rethrow exception
    throw $exception;
}
```

## Example 2. Method willThrowException()

We need emulate exception on calling `poll` method:

```php
try {
    $producerTopic->produce(RD_KAFKA_PARTITION_UA, 0, $message, $key);
    $this->producer->poll(0);

} catch (Throwable $exception) {
    $context['error'] = $exception->getMessage();
}
```

Test:

```php
$producer = self::createMock(Producer::class);
$producer->expects(self::once())
    ->method('poll')
    ->willThrowException($exception = new Exception('Some error message'));
```
