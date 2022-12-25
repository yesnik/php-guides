# PHP 8.2 Features

## Standalone types: null, true, and false

Before PHP 8.2 we could already use `false` with other types as union:

```php
function sayHi(): string|false {
	echo 'Hi';
	return false;
}
```

But now we can use `false`, `true`, `null` as a standalone type:

```php
function sayHi(): false {
	echo 'Hi';
	return false;
}

function sayHi(): true
{
	echo 'Hi';
	return true;
}

function sayHi(): null
{
	echo 'Hi';
	return null;
}
```

## Readonly classes

This syntactic sugar allows us to make all class properties readonly at once. So instead of writing this:

```php
class Student
{
    public function __construct(
        public readonly string $lastName, 
        public readonly string $firstName,
        public readonly DateTime $birthDate,
    ) {}
}
```
We can write this:

```php
readonly class Student
{
    public function __construct(
        public string $lastName, 
        public Author $firstName,
        public DateTime $birthDate,
    ) {}
}
```
It also prevents dynamic properties being added on a class.

### Readonly classes inheritance

We can extend from readonly classes, but we cannot change their readonly property. 

```php
readonly class A {}

class B extends A {}
// Fatal error: Non-readonly class B cannot extend readonly class A 
```
```php
class A {}

readonly class B extends A {}
// Fatal error: Readonly class B cannot extend non-readonly class A 
```
