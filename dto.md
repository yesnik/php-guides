# Data Transfer Object

```php
final class Item
{
    public function __construct(
        public readonly int $id,
        public readonly string $title,
    ) {}
}

$item = new Item(
    id: 1,
    title: 'Pencil',
);

// $item->id = 2; // Fatal error: Uncaught Error: Cannot modify readonly property Item::$id
echo $item->id; // 1
echo $item->title; // Pencil
```
