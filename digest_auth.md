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
$realm = 'Protected area';

$users = ['admin' => '123'];

if (empty($_SERVER['PHP_AUTH_DIGEST'])) {
    $nonce = uniqid();
    $opaque = md5($realm);

    header('HTTP/1.1 401 Unauthorized');
    header('WWW-Authenticate: Digest realm="'.$realm.
        '",qop="auth",nonce="'.$nonce.'",opaque="'.$opaque.'"');

    die('Error. No auth headers');
}

$data = $this->http_digest_parse($_SERVER['PHP_AUTH_DIGEST']);
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

$A1 = md5($username . ':' . $realm . ':' . $password);
$A2 = md5($_SERVER['REQUEST_METHOD'].':'.$data['uri']);
$valid_response = md5($A1.':'.$data['nonce'].':'.$data['nc'].':'.$data['cnonce'].':'.$data['qop'].':'.$A2);

echo 'Response: ' . $data['response'];

echo 'Valid response: ' . $valid_response;

if ($data['response'] !== $valid_response) {
    die('Invalid data!');
}

echo 'OK. You logged in as ' . $username;
```
