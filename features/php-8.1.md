# PHP 8.1 Features

## Enums

```php
enum Status {
  case Enabled;
  case Disabled;
}

class Cat {
    private Status $status;
    
    public function setStatus(Status $status)
    {
        $this->status = $status;
    }
    
    public function getStatus()
    {
        return $this->status;
    }
}
$tom = new Cat();
$tom->setStatus(Status::Enabled);
var_dump($tom->getStatus()); // enum(Status::Enabled)
```
```php
enum Status: int {
  case Enabled = 1;
  case Disabled = 0;
}

var_dump(Status::Enabled); // enum(Status::Enabled)
```

## Array unpacking with string keys

```php

$a = ['a' => 1];
$b = ['b' => 2];
$arr = ['a' => 0, ...$a, ...$b, 'c' => 3];

print_r($arr);
/*
Array
(
    [a] => 1
    [b] => 2
    [c] => 3
)
*/
```
