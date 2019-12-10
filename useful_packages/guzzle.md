# Guzzle PHP HTTP client

[Guzzle](https://github.com/guzzle/guzzle) is PHP HTTP client that makes it easy to send HTTP requests.

```bash
composer require guzzlehttp/guzzle
```

## GET query

```php
$client = new \GuzzleHttp\Client([
    'base_uri' => 'http://httpbin.org',
    'timeout'  => 2,
]);

$response = $client->get('/get');
```

### GET with timeout

```php
$client = new Client([
    'timeout'  => 2, // Timeout in seconds
]);

try {
    $response = $client->get('http://127.0.0.1:8000/get10sec');
} catch(\GuzzleHttp\Exception\ConnectException $e) {
    $response = ['success' => false, 'error' => $e->getMessage()];
}
```

## POST query

```php
$client = new \GuzzleHttp\Client([
    'timeout'  => 5,
]);

$response = $client->post('http://127.0.0.1:8000/home/postShow', [
    'query' => [
        'name' => 'Kenny',
        'age' => 18,
    ]
]);

$body = $response->getBody(); // GuzzleHttp\Psr7\Stream Object

$stringBody = $body->getContents();

print_r($stringBody);
```
