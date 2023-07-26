# PHP arrow functions

- An arrow function provides a shorter syntax for writing a short anonymous function.
- An arrow function starts with the `fn` keyword and contains only one expression, which is the returned value of the function.

```php
$users = [
    (new class() {
        public $id = 1;
    }),
    (new class() {
        public $id = 2;
    }),
];

$result = array_map(fn($user) => $user->id, $users);
print_r($result); // [1, 2]
```
```php
$res = array_map(
    fn($item) => $item * 2, [1, 2, 3]    
);
print_r($res); // [2, 4, 6]
```

## External context is accessible

An arrow function have the access to the variables in its parent scope automatically.

```php
$greet = 'Hi, ';
$hi = fn($name) => $greet . $name;

echo $hi('Kenny'); // Hi, Kenny
```
