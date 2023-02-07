# Sessions

See [docs](https://www.php.net/manual/en/intro.session.php)

Session support in PHP consists of a way to preserve certain data across subsequent accesses.

## Methods

### session_unset()

It frees all session variables currently registered. The use of `session_unset()` is identical to `$_SESSION = []`.

It's possible to assign new value after calling this function:

```php
session_unset();
// name will be saved to a session file
$_SESSION['name'] = 'After unset';
```

**Note:** Only use `session_unset()` for older deprecated code that does not use `$_SESSION`.

### session_destroy()

- It destroys all data registered to a session, including session file at `/var/lib/php/session`. 
- It's useless to work with `$_SESSION` after calling this method - data won't be saved.
- It doesn't clear `$_SESSION` variable.

```php
session_destroy();
// Session data is not empty
print_r($_SESSION);

// name won't be saved to session file
$_SESSION['name'] = 'After destroy';
```

## Example

File `index.php`:

```php
<?php
session_start();

if (!empty($_POST)) {
    switch ($_POST['action']) {
        case 'Save name':
            $_SESSION['name'] = $_POST['name'];
            break;

        case 'Reset Session':
            $_SESSION = [];
            setcookie(session_name(), '', 1, '/');
            session_destroy();
            break;
    }
    header('Location: ' . $_SERVER['HTTP_REFERER']);
}

?>

<h3>Session data</h3>
<pre>
<?php print_r($_SESSION); ?>
</pre>

<h3>Session config</h3>
<ul>
    <li>Use cookies: <?= ini_get('session.use_cookies') ?></li>
    <li>Session storage folder: <?= session_save_path() ?></li>
    <li>Sesson name: <?= session_name() ?></li>
    <li>Sesson id: <?= session_id() ?></li>
</ul>

<h3>Manage session</h3>
<form action="" method="post">
    <input type="text" name="name" placeholder="Enter name" />
    <input type="submit" name="action" value="Save name" />
    <p>
        <input type="submit" name="action" value="Reset Session" />
    </p>
</form>
```

## Sessions handle process

- See [session configuration](https://www.php.net/manual/en/session.configuration.php) params.
- Param `session.use_cookies` (default: 1) specifies whether the module will use cookies to store the session id on the client side.
- User opens a page in a browser.
- `session_start()` creates a session or resumes the current one based on a session identifier passed via a cookie (or via a GET or POST request).
- Name of this session is defined in param `session.name` (default: `PHPSESSID`).
- For this session PHP generates a unique ID (`3995nghgct24oopkg5mldd5vg7`) and write it to a cookie.
- Param `session.save_handler` (default: `files`) defines the name of the handler which is used for storing and retrieving data associated with a session.
- Session data is stored in `$_SESSION` variable. In PHP code we can read this variable or save values to it.
- PHP uses [serialize()](https://www.php.net/manual/en/function.serialize) to save `$_SESSION` to a file `/var/lib/php/session/sess_3995nghgct24oopkg5mldd5vg7`
- Here is a content of this file: `name|s:5:"Kenny";`
- PHP sends `PHPSESSID` with session ID value to browser via cookie.
- On next requests the browser will send `PHPSESSID` cookie to the server.
- PHP will search `PHPSESSID` value in the cookies' directory and compares it to the file name. 
  If both are the same, then it retrieves the existing session data, otherwise it creates a new session for that user.
- A session is destroyed when the user closes the browser. The server also terminates the session after the predetermined period of session time expires.
