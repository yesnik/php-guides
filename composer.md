# Composer for PHP apps

[Composer](https://getcomposer.org/download/) allows us to manage package dependencies.

## Commands

- `composer self-update` - Update composer's version
- `composer init` - Init project
- `composer dump-autoload` - Generate autoload files
- `composer -vvv install` - Install dependencies with verbose output (great for debugging)
- `composer install --no-dev` - Install dependencies without dev
- `composer req phpmd/phpmd --dev` - Add package 'phpmd/phpmd' to 'require-dev' of composer.json
- `composer req phpmailer/phpmailer 5.2.28` - Add/update package to specific version
- `composer remove phpmd/phpmd` - Remove package from composer.json
- `composer outdated --direct` - List required in composer.json packages that have updates available
- `composer why psr/container` - Show what packages use this package
- `composer why-not psr/container 2` - Show why project doesn't use version 2 of the package
- `composer why-not php 8`
- `composer depends symfony/polyfill-mbstring` - Show root dependencies that require this package
- `composer depends symfony/polyfill-mbstring --tree`
- `composer update` - Update minor dependencies
- `composer update --lock` - Update *content-hash* in composer.lock
- `composer update --with-dependencies phpmd/phpmd` - Update package with its dependencies
- `composer update "symfony/*"` - Update Symfony components
- `composer show` - Show all dependencies and versions
- `composer show --tree` - Show all dependencies as a tree 

## Autoload with composer

Init [composer](https://getcomposer.org/) in your project:

```
cd my_project
composer init
```

This command will generate file `composer.json`. 
Edit this file and add [autoload](https://getcomposer.org/doc/04-schema.md#psr-4) section:

```json
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

### autoload-dev

For development mode we also can add autoload section in the `composer.json`:

```
"autoload-dev": {
    "psr-4": {
        "tests\\": "tests/",
        "app\\": "./"
    }
},
```

## Config

```json
{
    "name": "kenny/app",
    "type": "project",
    "config": {
        "sort-packages": true
    },
    "require": {}
}
```

File `composer.json` has `config` section with many [options](https://getcomposer.org/doc/06-config.md):

- `sort-packages` - Defaults to false. If true, the require command keeps packages sorted by name in composer.json when adding a new package.

## Repositories

It's possible to download packages from your own storage (e.g. [Nexus](https://github.com/sonatype-nexus-community/nexus-repository-composer)) instead of default https://packagist.org

### Add repository

```
composer config repo.nexus composer https://nexus.somesite.com/repository/composer-proxy/
```

This command will create entry in `repositories` key at `composer.json` file:

```json
"repositories": {
    "nexus": {
        "type": "composer",
        "url": "https://nexus.somesite.com/repository/composer-proxy/"
    }
}
```

### Remove repository

```
composer config --unset repo.nexus
```

### Disable packagist

If you use your own package repository it's possible to disable usage of the default https://packagist.org:

```
composer config repo.packagist false
```

This command will add entry in `repositories` key at `composer.json` file:

```json
"repositories": {
    "nexus": {
        "type": "composer",
        "url": "https://nexus.somesite.com/repository/composer-proxy/"
    }
    "packagist": false
}
```
