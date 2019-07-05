# Request Params

We can pass different data to web server using different 
[HTTP methods](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) - GET, POST, PUT, DELETE, etc.
And PHP can handle request method and extract data from it.

## GET params

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

## POST params

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
