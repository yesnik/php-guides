# PHP Digest HTTP Authentication

See official PHP documentation about [Digest HTTP Authentication](https://www.php.net/manual/en/features.http-auth.php).

## Client script

```php
$url = 'http://site.com/api/callCenters/citycall/claims';

$username = 'admin';
$password = '123';

// $data = ['a' => 1];
// $postData = json_encode($data);

$ch = curl_init($url);
// curl_setopt($ch, CURLOPT_VERBOSE, 1);

curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 0);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, 0);
curl_setopt($ch, CURLOPT_TIMEOUT, 5); // Timeout in seconds

// curl_setopt($ch, CURLOPT_POST, 1);
// curl_setopt($ch, CURLOPT_POSTFIELDS, $postData);

// curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_DIGEST);
curl_setopt($ch, CURLOPT_USERPWD, $username . ":" . $password);

curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);

curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Accept: application/json',
    'Content-Type: application/json'
]);

$result = curl_exec($ch);
curl_close($ch);

print_r ($result);
```

## Server script

```php

// function to parse the http auth header
function http_digest_parse($txt)
{
    // protect against missing data
    $needed_parts = array('nonce'=>1, 'nc'=>1, 'cnonce'=>1, 'qop'=>1, 'username'=>1, 'uri'=>1, 'response'=>1);
    $data = array();
    $keys = implode('|', array_keys($needed_parts));

    preg_match_all('@(' . $keys . ')=(?:([\'"])([^\2]+?)\2|([^\s,]+))@', $txt, $matches, PREG_SET_ORDER);

    foreach ($matches as $m) {
        $data[$m[1]] = $m[3] ? $m[3] : $m[4];
        unset($needed_parts[$m[1]]);
    }

    return $needed_parts ? false : $data;
}

$realm = 'Protected area';

$users = ['admin' => '123'];

if (empty($_SERVER['PHP_AUTH_DIGEST'])) {
    $nonce = uniqid();
    $opaque = md5($realm);

    header('HTTP/1.1 401 Unauthorized');
    header('WWW-Authenticate: Digest realm="' . $realm .
        '",qop="auth",nonce="' . $nonce . '",opaque="' . $opaque . '"');

    die('Error. No auth headers');
}

echo '$_SERVER[PHP_AUTH_DIGEST]: ' . $_SERVER['PHP_AUTH_DIGEST'] . PHP_EOL;

$data = http_digest_parse($_SERVER['PHP_AUTH_DIGEST']);
echo 'Parsed $_SERVER[PHP_AUTH_DIGEST]:' . PHP_EOL;
print_r($data);
/*
Array
(
    [username] => admin
    [nonce] => 5e86ebdf56509
    [uri] => /api/callCenters/citycall/claims
    [qop] => auth
    [nc] => 00000001
    [cnonce] => 2cr6Oeoo
    [response] => f8dd0090e9e0ee7a9cf389689f914542
)
*/

$username = $data['username'];
$password = $users[$username];

// Note: you can store value $A1 in the Database. It'll help you not to store password in plain text.
$A1 = md5($username . ':' . $realm . ':' . $password);
$A2 = md5($_SERVER['REQUEST_METHOD'] . ':' . $data['uri']);
$valid_response = md5($A1 . ':' . $data['nonce'] . ':' . $data['nc'] . ':' . $data['cnonce'] . ':' . $data['qop'] . ':' . $A2);

echo 'Response: ' . $data['response'] . PHP_EOL;

echo 'Valid response: ' . $valid_response . PHP_EOL;

if ($data['response'] !== $valid_response) {
    die('Invalid data!');
}

echo 'OK. You logged in as ' . $username;
```

## Digest auth process

1. Client opens URL that executes PHP script (see *Server script* above).
2. Server sends headers to browser:
    ```
    HTTP/1.1 401 Unauthorized
    WWW-Authenticate: Digest realm="Protected area",qop="auth",nonce="6165463d918fa",opaque="3069d4b3ad68da81801066aecc0447e5"
    ```
3. Browser shows popup dialog to enter username and password. Let's enter username `admin`, password `123` and press *Enter* button.
4. Browser sends GET request to server with headers:
    ```
    GET /auth/index.php HTTP/1.1
    Authorization: Digest username="admin", realm="Protected area", nonce="6165470081840", uri="/", response="41b8d85ea999db083f1a2f67f22316c7", opaque="3069d4b3ad68da81801066aecc0447e5", qop=auth, nc=00000002, cnonce="c6ea54f0f87b1cf8"
    ```
6. Server accepts Authorization header in `$_SERVER['PHP_AUTH_DIGEST']` variable. It's stored there as a string.
7. Server script parses `Authorization` header and try to calculate `$valid_response` from provided parts.
8. If calculated `$valid_response` is equal to `response` received in Authorization header, then user provided correct password.
