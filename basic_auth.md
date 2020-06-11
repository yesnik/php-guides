# PHP Basic Auth

```php
// index.php
if (!isset($_SERVER['PHP_AUTH_USER'])) {
    header('WWW-Authenticate: Basic realm="My Realm"');
    header('HTTP/1.1 401 Unauthorized');
    echo 'Text to send if user hits Cancel button';
    exit;
} else {
    echo "<p>Hello {$_SERVER['PHP_AUTH_USER']}.</p>";
    echo "<p>You entered {$_SERVER['PHP_AUTH_PW']} as your password.</p>";
}
```

1. Try to open `index.php` in your browser. Server will respond:
    ```
    HTTP/1.1 401 Unauthorized
    WWW-Authenticate: Basic realm="My Realm"
    ...
    ```
2. Enter user `kenny` and password `123` in dialog box that browser displays, then press *Enter* button.
Browser will make the request:
    ```
    GET /index.php HTTP/1.1
    Authorization: Basic a2Vubnk6MTIz
    ...
    ```
    But what is the string `a2Vubnk6MTIz`? That's simple:

    ```php
    $user = 'kenny';
    $password = '123';

    $string = $user . ':' . $password;

    $encoded = base64_encode($string);
    echo $encoded; // 'a2Vubnk6MTIz'

    $decoded = base64_decode($encoded);
    echo $decoded; // 'kenny:123'
    ```
