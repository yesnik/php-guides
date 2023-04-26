# PHP arrow functions

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

## External context is accessible

```php
$greet = 'Hi, ';
$hi = fn($name) => $greet . $name;

echo $hi('Kenny'); // Hi, Kenny
```
