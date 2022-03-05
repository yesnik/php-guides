# Cookie

### Get cookies

Browser sends cookies to server in the HTTP-header:

```
Cookie  PHPSESSID=k6rnl7ckb19aba627r3omknigl; a=1
```

Server can read these values:

```php
print_r($_COOKIE);
/*
Array ( [PHPSESSID] => k6rnl7ckb19aba627r3omknigl [a] => 1 ) 
*/
```

### Set cookie value

Cookie `a` will expire in 24 hours:

```php
setcookie('a', 1, time() + 60 * 60 * 24);
```

Server sends cookie to the browser in the HTTP-header:

```
Set-Cookie	a=1; expires=Sun, 06-Mar-2022 13:39:25 GMT; Max-Age=86400
```

### Delete cookie value

Overwrite the current cookie with an empty string which expires one second after the epoch (1 January 1970 00:00:00 UTC):

```php
unset($_COOKIE['a']);
setcookie('a', '', 1);
```

Server will send the header to the browser:

```
Set-Cookie	a=deleted; expires=Thu, 01-Jan-1970 00:00:01 GMT; Max-Age=0
```
