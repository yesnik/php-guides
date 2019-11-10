# PHP Unit setup

1. Let's create a folder for our simple project:

```
mkdir world
```

2. Init composer 

```
cd calculator
composer init
```

You'll see file `composer.json` appeared in the directory.

3. Install `phpunit`:

```
composer require phpunit/phpunit --dev
```

4. Add `"autoload"` section to file `composer.json`:

```
{
    "name": "nik/world",
    "type": "project",
    "require": {},
    "require-dev": {
        "phpunit/phpunit": "^8.1"
    },
    "autoload": {
        "psr-4": { 
            "App\\": "src/"
        }
    }
}
```

5. Create config file for PHP Unit - `phpunit.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<phpunit verbose="true"
         colors="true"
         stopOnFailure="false"
         bootstrap="vendor/autoload.php"
>

    <testsuites>
        <testsuite name="Project Test Suite">
            <directory>tests</directory>
        </testsuite>
    </testsuites>

    <filter>
        <whitelist>
            <directory>src</directory>
        </whitelist>
    </filter>
</phpunit>
```

6. Create file `src/Services/World.php`:

```php
<?php

namespace App\Services;

class World {
    public function hello($name = 'stranger') {
        return 'Hello ' . $name;
    }
}
```

7. For our simple test create file `tests/Services/WorldTest.php`:

```php
<?php

use PHPUnit\Framework\TestCase;
use App\Services\World;

class WorldTest extends TestCase
{
    public function testHello()
    {
        $world = new World();

        $this->assertEquals('Hello Kenny', $world->hello('Kenny'));
    }
}
```

8. *Regenerate autoload files* of composer via command:

```
# Way 1
composer dump-autoload

# Way 2
composer install
```

9. Run tests: 

```
./vendor/bin/phpunit
```

We'll see results:

```
PHPUnit 8.4.3 by Sebastian Bergmann and contributors.

.                                                                   1 / 1 (100%)

Time: 113 ms, Memory: 4.00 MB

OK (1 test, 1 assertion)
```
