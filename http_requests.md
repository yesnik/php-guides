# HTTP Requests

We can make requests to web server using different 
[HTTP methods](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) - GET, POST, PUT, DELETE, etc.
And PHP can handle request method and extract data from it.

## GET request

Request: 

```bash
curl http://127.0.0.1:8000/?a=11&b=22
```

PHP:

```php
print_r( $_GET );

/* Output:
Array
(
    [a] => 11
    [b] => 22
)
*/
```

## POST request

Request:

```bash
curl -X POST -d 'a=1&b=2' "http://127.0.0.1:8000/"
```

PHP:

```php
print_r($_POST);

/* Output:
Array
(
    [a] => 1
    [b] => 2
)
*/
```

## PUT request

Request:

```bash
curl -X PUT -d 'a=1&b=2' "http://127.0.0.1:8000/"
```

PHP:

```php
print_r( file_get_contents('php://input') );

/* Output:
a=1&b=2
*/
```

## DELETE request

DELETE request doesn't contain body. PHP has info about incoming request method type.

Request:

```bash
curl -X DELETE http://127.0.0.1:8000/user/1
```

PHP:

```php
$method = $_SERVER['REQUEST_METHOD'];
echo $method;

/* Output:
DELETE
*/
```
