# Composer for PHP apps

Composer allows us to manage package dependencies.

## Autoload with composer

Init [composer](https://getcomposer.org/) in your project:

```
cd my_project
composer init
```

This command will generate file `composer.json`. 
Edit this file and add [autoload](https://getcomposer.org/doc/04-schema.md#psr-4) section:

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

require __DIR__ . '/vendor/autoload.php';

$tom = new Cat('Tom');
echo $tom->getNickname(); // 'Tom'
```
