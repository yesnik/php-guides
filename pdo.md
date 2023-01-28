# PDO

## Check connection to database

```php
<?php
$host = "127.0.0.1";
$dbName = 'my_app';
$username = "user";
$password = "password";

try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbName", $username, $password);
    // Set the PDO error mode to exception
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "Connected successfully";
} catch(PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
}
```
