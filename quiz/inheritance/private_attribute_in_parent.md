# Private attribute in parent class

```php
class Animal {
    private int $speed = 2;
    
    public function getSpeed(): int
    {
        return $this->speed;
    }
}

class Cat extends Animal
{
    public int $speed = 3;
}

$tom = new Cat();
echo $tom->getSpeed(); // ?
```

<details><summary>Answer</summary>
<b>PHP 8.2:</b> Method will return 2
</details>
