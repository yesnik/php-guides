# MySQLi

## SELECT

### No params in query

```php
<?php
$host = 'db';
$dbUser = 'mysite';
$dbPassword = 'password';
$dbName = 'mysite';

$mysqli = new mysqli($host, $dbUser, $dbPassword, $dbName);

if ($mysqli->connect_errno) {
    throw new RuntimeException('mysqli connection error: ' . $mysqli->connect_error);
}

// Set the desired charset after establishing a connection
$mysqli->set_charset('utf8mb4');
if ($mysqli->errno) {
    throw new RuntimeException('mysqli error: ' . $mysqli->error);
}

// Perform query
$result = $mysqli->query("SELECT * FROM users");
echo "Returned rows number: " . $result->num_rows . '<br>';

// Show rows
foreach ($result as $row) {
    echo $row['id'] . ' ' . $row['name'] . '<br>';
}

$result->free_result();
$mysqli->close();
```

### With params in query

[mysqli::prepare](https://www.php.net/manual/en/mysqli.prepare.php) prepares the SQL query, and returns a statement handle to be used for further operations on the statement. The query must consist of a single SQL statement.

The statement template can contain zero or more question mark `?` parameter markers⁠—also called *placeholders*. 
The parameter markers must be bound to application variables using [mysqli_stmt_bind_param()](https://www.php.net/manual/en/mysqli-stmt.bind-param.php) before executing the statement.

```php
$postalCode = 47063;

$sql = 'SELECT * FROM customers WHERE postal_code = ?';

$stmt = $mysqli->prepare($sql);
$stmt->bind_param('i', $postalCode);
$stmt->execute();

$result = $stmt->get_result();

foreach ($result as $row) {
    echo $row['id'] . ' ' . $row['name'] . '<br>';
}
```
