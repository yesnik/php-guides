# PHP Inheritance

## Method of child class can be less private

```php
class Animal
{
    protected function getTitle() {}
}

class Cat extends Animal
{
    // We can use 'public' instead of 'protected' here.
    public function getTitle() {
        return 'Cat';
    }
}

echo (new Cat)->getTitle(); // 'Cat'
```
