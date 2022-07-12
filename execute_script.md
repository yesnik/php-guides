# Execute script

```php
function execScript(string $script)
{
    $output = null;
    $code = null;
    exec($script, $output, $code);
    if ($code !== 0) {
        throw new Exception("Error occured in script: '$script'");
    }
}
```

## Import dump to database

```php
$user = 'root';
$password = 'password';
$dbName = 'sales_test';
$path = '/var/tmp/db_dump.sql';

execScript("mysqldump --single-transaction --no-data -u {$user} -p{$password} {$dbName} > {$path}");
```
