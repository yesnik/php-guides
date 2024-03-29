# Curl in PHP

Here you can find examples of usage `curl` in PHP.

## GET request

```php
$ch = curl_init();

$url = "https://www.google.com/";

curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_TIMEOUT, 5); // Timeout in seconds

$response = curl_exec($ch);

// if the CURLOPT_RETURNTRANSFER option is set, curl_exec will return false on failure
if ($response === false) {
    echo 'Error code: ' . curl_errno($ch) . ', error: ' . curl_error($ch);
    die();
}

$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);

echo $httpCode;
echo $response;

curl_close($ch);
```

## POST JSON

```php
$url = 'https://google.com';
$dataStr = json_encode(['a' => 1]);

$ch = curl_init($url);

curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $dataStr);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
// curl_setopt($ch, CURLOPT_VERBOSE, true); // For debug
curl_setopt($ch, CURLOPT_TIMEOUT_MS, 2000);
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Content-Type: application/json',
    'Accept: application/json',
    'Content-Length: ' . strlen($dataStr)
]);

$content = curl_exec($ch);
if (!$content) {
    throw new Exception(curl_error($ch), curl_errno($ch));
}

echo 'Response code: ' . curl_getinfo($ch, CURLINFO_RESPONSE_CODE) . PHP_EOL;
echo 'Response content: ' . $content;

curl_close($ch);
```

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


$curlError = '';
// This condition will help us to catch timeout error
if (curl_errno($ch)) {
    $curlError = curl_error($ch);
}

curl_close($ch);

echo PHP_EOL . "Curl error:" . PHP_EOL;
echo $curlError;

echo PHP_EOL . "Response code:" . PHP_EOL;
print_r($httpCode);

echo PHP_EOL . "Result:" . PHP_EOL;
print_r ($result);
```
