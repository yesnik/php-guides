# PHP autoload classes

In PHP there is an instrument to autoload classes that script sees at first time. 


## Before using autoload

Before using the class we need to include it in the script.

File `index.php`:

```php
<?php

include __DIR__ . '/classes/Cat.php';
include __DIR__ . '/classes/Dog.php';
include __DIR__ . '/classes/Cow.php';
include __DIR__ . '/libs/Calculator.php';

$tom = new Cat('Tom');
echo $tom->getNickname(); // 'Tom'

$calculator = new Calculator();
echo $calculator->add(1, 2); // 3
```

## After using autoload

We don't need to `require` each file manually. All we need is to register at least one autoload function.

File `index.php`:

```php
<?php

// We can create named function for autoload
function loadFromClasses($className) {
  include __DIR__ . '/classes/' . $className . '.php';
}

spl_autoload_register('loadFromClasses');

// We can define autoload logic in anonymous function
spl_autoload_register(function($className) {
  include __DIR__ . '/libs/' . $className . '.php';
});

$tom = new Cat('Tom');
echo $tom->getNickname(); // 'Tom'

$calculator = new Calculator();
echo $calculator->add(1, 2); // 3

// We can display all autoload functions
print_r( spl_autoload_functions() );

// We can unregister autoload function
spl_autoload_unregister('loadFromClasses');

$druppy = new Dog('Druppy'); // Fatal error: Uncaught Error: Class 'Dog' not found
```

## Autoload with composer

Init [composer](https://getcomposer.org/) in your project:

```
cd my_project
composer init
```

This command will generate file `composer.json`. Edit this file and add [autoload](https://getcomposer.org/doc/04-schema.md#psr-4) section:

```
{
    "name": "nik/myautoload",
    "description": "Autoload test",
    "type": "project",
    "require": {},
    "autoload": {
        "psr-4": { "": [
          "classes/",
          "libs/"
          ] 
        }
    }
}
```

Include composer's autoload file to your script. File `index.php`:

```php
<?php

require_once 'vendor/autoload.php';

$tom = new Cat('Tom');
echo $tom->getNickname(); // 'Tom'
```
