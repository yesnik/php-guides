# Composer for PHP apps

Composer allows us to manage package dependencies.

## Commands

```bash
# Init project
composer init

# Add package 'phpmd/phpmd' to 'require-dev' of composer.json
composer req phpmd/phpmd --dev

# Remove package from composer.json
composer remove phpmd/phpmd

# Install dependencies without dev
composer install --no-dev

# Generate autoload files
composer dump-autoload

# Update package to specific version
composer req phpmailer/phpmailer 5.2.28

# Shows a list of installed packages that have updates available
composer outdated

# Update Symfony components
composer update "symfony/*"

# Show all dependencies as a tree 
composer show --tree
```

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
        "psr-4": {
            "App\\": "src/"
        }
    }
}
```

Include composer's autoload file to your script. File `index.php`:

```php
<?php

require __DIR__ . '/vendor/autoload.php';

// Your code here...

use App\Animals\Cat;

$tom = new Cat();
```
