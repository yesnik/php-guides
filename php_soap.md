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

## SOAP Request via curl

```php
<?php

$username = 'admin';
$password = '123';

$xmlData = <<<XML
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="URN:DOWNLOAD_DIRECTORIES">
   <soapenv:Header/>
   <soapenv:Body>
      <urn:MT_REQ_DIRECTORIES>
         <REQUEST type="GET_BANK_PRODUCTS_TO_POINTS_OF_SALES">
<PRODUCT>P105</PRODUCT>
         </REQUEST>
      </urn:MT_REQ_DIRECTORIES>
   </soapenv:Body>
</soapenv:Envelope>
XML;

// We can get this URL from WSDL file
$URL = "http://test.mysite.ru:5000/XISOAPAdapter/MessageServlet?channel=:DOWNLOAD_DIRECTORIES:SEND_SOAP&version=3.0&Sender.Service=DOWNLOAD_DIRECTORIES&Interface=URN:DOWNLOAD_DIRECTORIES^SI_REQ_DIRECTORIES";

$ch = curl_init($URL);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: text/xml'));
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $xmlData);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
curl_setopt($ch, CURLOPT_USERPWD, $username . ":" . $password);

$output = curl_exec($ch);
curl_close($ch);

print_r($output);
```
