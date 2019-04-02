# Curl in PHP

Here you can find examples of usage `curl` in PHP.

## POST JSON with digest auth

```php
$url = 'https://hello.com/callinglist/add/';
$username = 'kenny';
$password = '123';

$claim = [
    'phone' => '9111111111',
    'first_name' => 'Kenny'
];

$postData = json_encode($claim);

$ch = curl_init($url);

// curl_setopt($ch, CURLOPT_VERBOSE, 1);

curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 0);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, 0);
curl_setopt($ch, CURLOPT_TIMEOUT, 5); // Timeout in seconds

curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $postData);

//curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_DIGEST);
curl_setopt($ch, CURLOPT_USERPWD, $username . ":" . $password);

curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);

curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Accept: application/json',
    'Content-Type: application/json'
]);

$result = curl_exec($ch);
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);

// This condition will help us to catch timeout error
if (curl_errno($ch)) {
    echo PHP_EOL . "Curl error:" . PHP_EOL;
    echo curl_error($ch);
}

curl_close($ch);

echo PHP_EOL . "Response code:" . PHP_EOL;
print_r($httpCode);

echo PHP_EOL . "Result:" . PHP_EOL;
print_r ($result);
```
