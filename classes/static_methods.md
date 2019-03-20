# Static class methods in php

## Protected static methods

In php we can call protected static method from public static method.

```php
class Car {
    public static function getTitle()
    {
        return 'Audi ' . self::getYear();
    }
    
    protected static function getYear() {
        return '2017';
    }
}

echo Car::getTitle(); // 'Audi 2017'
```

**Note:** we cannot call protected static method directly:
```php
Car::getYear(); // Fatal error:  Call to protected method Car::getYear() from context '' in ...
```

## new self vs. new static

- `new self()` - will be executed in the context of the class whom it physically belongs to
- `new static()` - will be executed in the context of child class

```php
class Animal {
    public static function getSelf() {
        return new self();
    }

    public static function getStatic() {
        return new static();
    }
}

class Cat extends Animal {}

echo get_class(Cat::getStatic()); // Cat
echo get_class(Cat::getSelf());  // Animal

echo get_class(Animal::getStatic()); // Animal
echo get_class(Animal::getSelf()); // Animal
```

## Inheritance of static methods

Word `static` helps us to redefine method in subclass.

```php
class Robot {
    public static function info()
    {
        return self::getPowers();
    }
    
    public static function info2()
    {
        return static::getPowers();
    }
    
    protected static function getPowers()
    {
        return ['move'];
    }
}

class Robocop extends Robot {
    protected static function getPowers()
    {
        return ['speak'];
    }
}

print_r(Robocop::info()); // ['move']
print_r(Robocop::info2()); // ['speak']
```
