# PHP Unit setup

1. Let's create a folder for our simple project:

```
mkdir calculator
```

2. Init composer 

```
cd calculator
composer init
```

You'll see file `composer.json` in the directory.

3. Install `phpunit` and `phpunit-bridge`:

```
composer require phpunit/phpunit --dev
composer require symfony/phpunit-bridge --dev
```

4. Add `"autoload"` section to file `composer.json`:

```
{
    "name": "nik/calculator",
    "type": "project",
    "require": {},
    "require-dev": {
        "symfony/phpunit-bridge": "^4.2",
        "phpunit/phpunit": "^8.1"
    },
    "autoload": {
        "psr-4": { 
            "App\\": "src/"
        }
    }
}
```

5. Create file `config/bootstrap.php`:

```php
<?php

require dirname(__DIR__).'/vendor/autoload.php';
```

6. Create config file for PHP Unit - `phpunit.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- https://phpunit.de/manual/current/en/appendixes.configuration.html -->
<phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://schema.phpunit.de/6.5/phpunit.xsd"
         backupGlobals="false"
         colors="true"
         bootstrap="config/bootstrap.php"
>
    <php>
        <ini name="error_reporting" value="-1" />
        <server name="APP_ENV" value="test" force="true" />
        <server name="SHELL_VERBOSITY" value="-1" />
    </php>

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

    <listeners>
        <listener class="Symfony\Bridge\PhpUnit\SymfonyTestsListener" />
    </listeners>
</phpunit>
```

7. Create file `src/Services/Calculator.php`:

```php
<?php

namespace App\Services;

class Calculator {
    public function add($a, $b) {
        return $a + $b;
    }
}
```

8. For our simple test create file `tests/Services/CalculatorTest.php`:

```php
<?php

use PHPUnit\Framework\TestCase;
use App\Services\Calculator;

class CalculatorTest extends TestCase
{
    public function testEquals()
    {
        $calculator = new Calculator();

        $this->assertEquals(4, $calculator->add(1, 3));
    }
}
```

9. Regenerate autoload files via command: `composer install`

10. Run tests: `./vendor/bin/phpunit`
