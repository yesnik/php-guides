# Magic methods in PHP

PHP class has methods that will be called if attribute is not defined or not public.

```php
<?php
class Cat {
    public $name;
    protected $nameSecret;
    
    public function getSecretName()
    {
        $this->nameSecret;
    }
    
    protected function think()
    {
        echo 'Thinking...';
    }
    
    public function __get($attr)
    {
        echo 'Attribute called: ' . $attr . PHP_EOL;
    }
    
    public function __set($attr, $val)
    {
        echo "Set attribute: {$attr}, val: {$val}" . PHP_EOL;
    }
    
    public function __isset($attr)
    {
        echo 'isset was called for attribute: ' . $attr . PHP_EOL;
    }
    
    public function __unset($attr)
    {
        echo 'unset was called for attribute: ' . $attr . PHP_EOL;
    }
    
    public function __call($attr, $args)
    {
        echo "Attribute {$attr} was called with args: " . print_r($args, true) . PHP_EOL;
    }
}

$tom = new Cat;
$tom->name;
$tom->nameSecret; // => Attribute called: nameSecret
$tom->aaa; // => Attribute called: age

$tom->name = 'Tom';
$tom->color = 'gray'; // Set attribute: color, val: gray

isset($tom->name);
isset($tom->nameSecret); // isset was called for attribute: nameSecret
isset($tom->aaa); // isset was called for attribute: aaa

unset($tom->name);
unset($tom->bbb); // unset was called for attribute: bbb

$tom->getSecretName();
$tom->think(); // Attribute think was called with args: Array()
$tom->jump('table', 'bed'); // Attribute jump was called with args: Array([0] => table [1] => bed)
```
