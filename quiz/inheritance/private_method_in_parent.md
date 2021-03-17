# Private method in parent class

```php
class Animal {
    private function getSpeed(): int
    {
        return 2;
    }
    
    public function move(): string
    {
        return 'Moving at speed = ' . $this->getSpeed();
    }
}

class Cat extends Animal
{
    public function getSpeed(): int
    {
        return 3;
    }
}

$tom = new Cat();
echo $tom->move(); // ?
```
