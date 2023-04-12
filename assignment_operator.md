# Assignment Operator

## Copy array by value

Array `$two` won't be modified:

```php
$one = [
	'a' => 1,
	'b' => 2,
];

$two = $one;

$one['a'] = 11;

print_r($one);
/* Array (
    [a] => 11
    [b] => 2
)
*/

print_r($two);
/* Array (
    [a] => 1
    [b] => 2
)
*/
```

## Copy object by reference

```php
class Cat
{
    public $name;
    public $age;
    
    public function __construct(string $name, int $age)
    {
        $this->name = $name;
        $this->age = $age;
    }
}

$kesha = new Cat('Kesha', 5);

$tom = $kesha;

$tom->name = 'Tom';

print_r($kesha);
print_r($tom);
/* Both will print:
Cat Object
(
    [name] => Tom
    [age] => 5
)
*/
```
