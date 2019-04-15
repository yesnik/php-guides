# Closure in PHP

Class used to represent anonymous functions. See [documentation](https://www.php.net/manual/en/class.closure.php)

```php
function run ($fn) {
    var_dump($fn); // object(Closure)#1
    
    if ($fn instanceof Closure) {
        echo 'Closure here!';
    }
    
    return $fn();
}

$hello = function (){
    return 'hello';
};
echo run($hello);
```
