# PDO

## Check connection to database

### MySQL

```php
<?php
$host = "127.0.0.1";
$dbName = 'my_app';
$username = "user";
$password = "password";

try {
    $pdo = new PDO("mysql:host=$host;dbname=$dbName", $username, $password);
    // Set the PDO error mode to exception
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "Connected successfully";
} catch(PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
}
```

### PostgreSQL

```php
$pdo = new \PDO('pgsql:host=database;port=5432;user=app;password=123');

echo $pdo->query('SELECT CURRENT_DATABASE()')
    ->fetch(\PDO::FETCH_COLUMN), PHP_EOL;
```

## Select

```php
$sql = 'SELECT id, alias FROM products ORDER BY id';
$result = $pdo->query($sql);
foreach ($result as $row) {
    echo $row['id'] . ' - ' . $row['alias'] . '<br>';
}
```

### With params

```php
$sql = 'SELECT id, alias FROM products WHERE status = :status';
$sth = $pdo->prepare($sql);
$sth->execute(['status' => 1]);
$result = $sth->fetchAll();
foreach ($result as $row) {
    echo $row['id'] . ' - ' . $row['alias'] . '<br>';
}
```

### Map result to a class with `PDO::FETCH_CLASS`

```php
$host = '127.0.0.1';
$db = 'my_store';
$user = 'root';
$password = '';
$dsn = "mysql:host=$host;dbname=$db";
$pdo = new PDO($dsn, $user, $password);

$stmt = $pdo->query('SELECT * FROM products');
$stmt->setFetchMode(PDO::FETCH_CLASS, Product::class);
$products = $stmt->fetchAll();
```
