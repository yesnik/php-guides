# Exception tests

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
