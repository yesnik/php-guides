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

## Make SOAP request

We have WSDL file, and we want to send this XML to the service:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="URN:DOWNLOAD_DIRECTORIES">
   <soapenv:Body>
      <urn:MT_REQ_DIRECTORIES>
         <REQUEST type="GET_BANK_PRODUCTS_TO_POINTS_OF_SALES">
            <PRODUCT>P105</PRODUCT>
         </REQUEST>
      </urn:MT_REQ_DIRECTORIES>
   </soapenv:Body>
</soapenv:Envelope>
```

### With PHP SoapClient

**Get available functions of the service**

See *full example* below. It's part of it:

```php
print_r($client->__getFunctions());
/* Returns:
Array
(
    [0] => DT_RES_DIRECTORIES SI_REQ_DIRECTORIES(DT_REQ_DIRECTORIES $MT_REQ_DIRECTORIES)
)
*/
```

It means that our service has only one function: `SI_REQ_DIRECTORIES`. This function:

- accepts argument `$MT_REQ_DIRECTORIES` of the type `DT_REQ_DIRECTORIES`
- returns value of the type `DT_RES_DIRECTORIES`

**Get types of the service**

See *full example* below. It's part of it:

```php
print_r($client->__getTypes());
/* Returns:
Array
(
    [0] => struct DT_RES_DIRECTORIES {
 POINT_SALES POINT_SALES;
 BANK_PRODUCTS_TO_POINTS_OF_SALES BANK_PRODUCTS_TO_POINTS_OF_SALES;
 ...
}
...
    [11] => struct DT_REQ_DIRECTORIES {
 REQUEST REQUEST;
}
    [12] => struct REQUEST {
 string PRODUCT;
 string type;
}
*/
```

It means that the type `DT_REQ_DIRECTORIES` requires `REQUEST` tag. `REQUEST` tag must have 2 params: `PRODUCT` and `type`.

**Full example**

```php
ini_set('soap.wsdl_cache_ttl', 0);

// Timeout for request in seconds
ini_set('default_socket_timeout', 3);

$wsdlPath = __DIR__ . '/wsdl/PIQ_SI_REQ_DIRECTORIES.wsdl';

$client = new SoapClient($wsdlPath, [
    'soap_version' => SOAP_1_1,
    'trace' => 1,
    'cache_wsdl' => WSDL_CACHE_NONE,
    'login' => 'admin',
    'password' => '123'
]);

// echo 'Types: ' . PHP_EOL;
// print_r($client->__getTypes());

// echo 'Get functions of the service: ' . PHP_EOL;
// print_r($client->__getFunctions());
// echo PHP_EOL . PHP_EOL;

$data = [
    'REQUEST' => [
        'PRODUCT' => 'P105',
        'type' => 'GET_BANK_PRODUCTS_TO_POINTS_OF_SALES',
    ],
];

class SoapTimeoutException extends Exception {};

try {
    $response = $client->__soapCall("SI_REQ_DIRECTORIES", [$data]);

    var_dump($response);

} catch (Throwable $exception) {
    $message = $exception->getMessage();

    if ($message === 'Error Fetching http headers') {
        throw new SoapTimeoutException();
    }

    echo 'ERROR! ';
    print_r($client->__getLastResponse());
    
    echo 'REQUEST HEADERS ';
    print_r($client->__getLastRequestHeaders());

    echo 'REQUEST';
    echo htmlentities($this->soapClient->__getLastRequest());

    throw $exception;
}
```

### With curl

```php
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

var_dump($output);
```
