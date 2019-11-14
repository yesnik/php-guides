# Predefined interfaces in PHP

See [docs](https://www.php.net/manual/en/reserved.interfaces.php).

## IteratorAggregate

Implement this interface to pass object of the class to `foreach` method.

```php
class Collection implements IteratorAggregate
{
    protected $items;

    public function __construct(array $items = [])
    {
        $this->items = $items;
    }

    public function getIterator()
    {
        return new \ArrayIterator($this->items);
    }
}

$items = new Collection([1, 2, 3]);

foreach($items as $item) {
    echo $item;
}
// Output: 123
```
