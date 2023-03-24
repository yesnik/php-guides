# Console commands

### Run tests

```bash
./vendor/bin/phpunit tests/

# Output test results grouped by class and method name
./vendor/bin/phpunit --testdox

# Output verbose info
./vendor/bin/phpunit --debug

# Infinite memory limit
./vendor/bin/phpunit -d memory_limit=-1 --testsuite=sales --no-coverage --filter=ApiLifeMediaTest
```

### Run only one method

```bash
vendor/bin/phpunit tests/MyTest.php --filter 'myMethodName'
```
