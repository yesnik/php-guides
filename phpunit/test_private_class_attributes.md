# PHPUnit test private class attributes

## Example

```php
/** @test */
public function product_id_is_integer()
{
    $product = new Product;

    $reflector = new ReflectionClass(Product::class);

    $property = $reflector->getProperty('product_id');
    $property->setAccessible(true);

    $value = $property->getValue($product);

    $this->assertIsInt($value);
}
```
