# Sessions

See [docs](https://www.php.net/manual/en/intro.session.php)

Session support in PHP consists of a way to preserve certain data across subsequent accesses.

## Example

```php
// File: index.php

session_start();

if (isset($_SESSION['name'])) {
    echo 'Hello, ' . $_SESSION['name'];
} else {
    echo 'No name';
    $_SESSION['name'] = 'Kenny';
}

// Show sessions config
echo 'Use cookies: ' . ini_get('session.use_cookies') . '<br>';
echo 'Sessions are stored at: ' . session_save_path() . '<br>';
echo 'Sesson name: ' . session_name() . '<br>';
```

## Sessions handle process

- See [session configuration](https://www.php.net/manual/en/session.configuration.php) params.
- Param `session.use_cookies` (default: 1) specifies whether the module will use cookies to store the session id on the client side.
- User opens a page in a browser.
- `session_start()` creates a session or resumes the current one based on a session identifier passed via a GET or POST request, or passed via a cookie.
- Name of this session is defined in param `session.name` (default: `PHPSESSID`).
- For this session PHP generates a unique ID (`3995nghgct24oopkg5mldd5vg7`) and write it to a cookie.
- Param `session.save_handler` (default: `files`) defines the name of the handler which is used for storing and retrieving data associated with a session.
- In our case session data is written to a file `/var/lib/php/session/sess_3995nghgct24oopkg5mldd5vg7`
- PHP sends `PHPSESSID` with session ID value to browser via cookie.
- On next requests the browser will send `PHPSESSID` cookie to the server.
- PHP will search `PHPSESSID` value in the temporary directory and compares it to the file name. 
  If both are the same, then it retrieves the existing session data, otherwise it creates a new session for that user.
- A session is destroyed when the user closes the browser. The server also terminates the session after the predetermined period of session time expires.
