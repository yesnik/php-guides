# PHP SOAP

## Install php-soap extension on server

```bash
sudo yum install php-soap
```

Check `php-soap` extension installation:

```bash
php -m | grep soap
```
If this command returns `soap` then you have installed `php-soap` extension on the server.

## Example of PHP SOAP client

```php
<?php

ini_set('soap.wsdl_cache_ttl', 0);
ini_set("default_socket_timeout", 20);

$url = dirname(__DIR__) . '/wsdl/PIQ_SI.wsdl';

$client = new SoapClient($url, [
    'soap_version' => SOAP_1_1,
    'trace' => 1,
    'cache_wsdl' => WSDL_CACHE_NONE,
    'login' => 'admin',
    'password' => '123'
]);

//var_dump($client->__getTypes());

// Returns all methods of service
//var_dump($client->__getFunctions());

$data = [
    'GENERAL' => [
        'IDENTIDIER' => 'S11',
        'ID' => '800',
    ],
    'BP' => [
        'TEL_NUMBER' => '(911)1111111'
    ],
    'TASK' => [
        'PROCESS_TYPE' => 'ZINT',
        'DATE_FROM' => '2019-03-21',
        'TIME_FROM' => '10:40:05.0Z',
    ]
];

class SoapTimeoutException extends Exception {};

try {
    $response = $client->__soapCall("SI_REQ", [$data]);

    var_dump($response);

} catch (SoapFault $exception) {
    $message = $exception->getMessage();

    if ($message === 'Error Fetching http headers') {
        throw new SoapTimeoutException();
    }
    
} catch (Exception $exception) {
    echo 'ERROR! ';
    print_r($client->__getLastResponse());
    echo 'REQUEST HEADERS ';
    print_r($client->__getLastRequestHeaders());

    echo 'REQUEST';
    print_r($client->__getLastRequest());

    throw $exception;
}
```
