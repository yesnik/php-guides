# Console commands

### Run tests

```bash
./vendor/bin/phpunit tests/

# Verbose output
./vendor/bin/phpunit --testdox

# Infinite memory limit
./vendor/bin/phpunit -d memory_limit=-1 --testsuite=sales --no-coverage --filter=ApiLifeMediaTest
```

### Run only one method

```bash
vendor/bin/phpunit tests/MyTest.php --filter 'myMethodName'
```
