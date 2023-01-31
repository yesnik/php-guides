# PHP variables

## Superglobals

- `$GLOBALS`. An array of all global variables. It's a global keyword, that allows you to
access global variables inside a function: `$GLOBALS['myvariable']`
- `$_GET`. An array of variables passed to the script via the GET method
- `$_POST`. An array of variables passed to the script via the POST method
- `$_COOKIE`. An array of cookie variables
- `$_REQUEST`. An array of all user input including the contents of input including `$_GET`, `$_POST`, `$_COOKIE` (but not including `$_FILES`)
- `$_FILES`. An array of variables related to file uploads
- `$_SERVER`. An array of server environment variables
  - `$_SERVER['DOCUMENT_ROOT']` - eg. */var/www/html*
  - `$_SERVER['REMOTE_ADDR']` - eg. *215.11.23.11*
  - `$_SERVER['REMOTE_PORT']` - eg. *56093*
  - `$_SERVER['REQUEST_URI']` - eg. */user/?name=johny&age=16*
  - `$_SERVER['QUERY_STRING']` - eg. *name=johny&age=16*
  - `$_SERVER['REQUEST_METHOD']` - eg. *POST*
- `$_ENV`. An array of environment variables
- `$_SESSION`. An array of session variables

### get_defined_vars

```php
print_r(get_defined_vars());
```

## Scopes

### Assignment inside function call

```php
function hi($name) {
    return 'Hi, ' . $name;
}

echo hello(
    $user = 'Kenny'    
);

echo $user; // 'Kenny'
```
