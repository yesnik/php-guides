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
- `$_ENV`. An array of environment variables
- `$_SESSION`. An array of session variables
