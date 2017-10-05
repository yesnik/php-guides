# flock()

We can use php function `flock()` to ensure that only one instance of script is being executed at any moment of time.

```php
$fileLock = '/var/tmp/some_file.lock';

$fp = fopen ($fileLock, 'w');

if ($fp === false ) {
    throw new Exception('Cannot create or open lock file');
}

if (flock($fp, LOCK_EX | LOCK_NB ) === false ) {
    echo 'Process is running already';
    die();
}

echo "Execute our task...";

sleep (5);

echo 'Lock was released';
fclose ($fp);
```
