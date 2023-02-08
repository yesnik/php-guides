# MySQLi

## SELECT

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
