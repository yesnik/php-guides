# PHP Code Quality Tools

Use [FIG](https://www.php-fig.org/) Framework Interoperability Group in your projects.

Within software development Lehman (1985) suggested a number of laws, of which two were, basically, as follows:

- A computer program that is used will be modified
- When a program is modified, its complexity will increase, provided that one does not actively work against this.

Andrew Hunt and David Thomas use fixing *broken windows* as a metaphor for avoiding *software entropy* in software development.

The process of code refactoring can result in stepwise reductions in *software entropy*.

Software entropy is increased with accumulation of technical debt.

### PHP-CS-Fixer

[PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer) is a tool to automatically fix PHP Coding Standards issues.
With the following command you can format an entire codebase:

```bash
php-cs-fixer fix src/
```

### PHP CodeSniffer

[PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) tokenizes PHP, JavaScript and CSS files and detects violations of a defined set of coding standards.

Output the actual coding standards flaws:

```bash
phpcs src/ --standard=PSR12
```

Fix some errors for you:

```bash
phpcbf src/Model/Calculator.php --standard=PSR12
```

### PHP Mess Detector

[PHPMD](https://github.com/phpmd/phpmd) is a spin-off project of PHP Depend and aims to be a PHP equivalent of the well known Java tool PMD. PHPMD can be seen as an user friendly frontend application for the raw metrics stream measured by PHP Depend.

PHPMD will display the possible bugs and misuses of the language in your application:

```bash
phpmd src/ text cleancode
```

PHPMD will scan the directory and sub-directories of your project and output in plain text the errors found. 
You can as well create an `html` or `xml` output by replacing the text option in the command line above.

### PHP Stan

[PHP Stan](https://github.com/phpstan/phpstan) - PHP Static Analysis Tool - discover bugs in your code without running it.
It's a good complement to PHPMD.

```bash
phpstan analyse src/ --level=7
```

You can precise the strictness of PHPStan with the `level` option. The minimum is `level` 0, the maximum level 7.

### PHP Loc

[phploc](https://github.com/sebastianbergmann/phploc) is a tool for quickly measuring the size and analyzing the structure of a PHP project.

```bash
phploc src
```

### PHP Insights

[PHP Insights](https://github.com/nunomaduro/phpinsights) was carefully crafted to simplify the analysis of your code directly from your terminal, and is the perfect starting point to analyze the code quality of your PHP projects.

```bash
./vendor/bin/phpinsights ./src
```

### PHP Copy/Paste Detector

[phpcpd](https://github.com/sebastianbergmann/phpcpd) is a Copy/Paste Detector (CPD) for PHP code.

```bash
phpcpd src/
```

### dePHPend

[dephpend](https://github.com/mihaeu/dephpend) detects flaws in your architecture, before they drag you down into the depths of dependency hell.

### churn-php

[churn-php](https://github.com/bmitch/churn-php) is a package that helps you identify php files in your project that *could be good candidates for refactoring*. It examines each PHP file in the path it is provided and:

- Checks how many commits it has.
- Calculates the cyclomatic complexity.
- Creates a score based on these two values.

### PHP Metrics

[PhpMetrics](https://github.com/phpmetrics/PhpMetrics) provides metrics about PHP project and classes, with beautiful and readable HTML report.

```bash
phpmetrics --report-html=myreport.html src/
```

### Security tests: Sensiolabs

[security-checker](https://github.com/sensiolabs/security-checker) is a command line tool that checks if your application uses dependencies with known security vulnerabilities.

```bash
composer require sensiolabs/security-checker --dev
```

```bash
php security-checker security:check /path/to/composer.lock
```
