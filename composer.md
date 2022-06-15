# Composer for PHP apps

[Composer](https://getcomposer.org/download/) allows us to manage package dependencies.

## Commands

```bash
# Update composer's version
composer self-update

# Init project
composer init

# Add package 'phpmd/phpmd' to 'require-dev' of composer.json
composer req phpmd/phpmd --dev

# Add/update package to specific version
composer req phpmailer/phpmailer 5.2.28

# Remove package from composer.json
composer remove phpmd/phpmd

# Install dependencies without dev
composer install --no-dev

# Install dependencies with verbose output (great for debugging)
composer -vvv install

# Generate autoload files
composer dump-autoload

# List required in composer.json packages that have updates available
composer outdated --direct

# Show what packages use this package
composer why psr/container

# Show why project doesn't use version 2 of the package
composer why-not psr/container 2
composer why-not php 8

# Update minor dependencies
composer update

# Update package with its dependencies
composer update --with-dependencies phpmd/phpmd

# Update Symfony components
composer update "symfony/*"

# Show all dependencies and versions
composer show

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
