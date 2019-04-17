# PHP script user info

## Get the name of script runner

```php
echo(exec("whoami"));
```

## Gets the name of the owner of the current PHP script

```php
get_current_user();
```

## Get system id of script's runner

Place this in your php-script to define `uid` of user that runs it:

```php
$userId = posix_getuid();
var_dump($userId); // 996
```

## Return info about a user by user id

```php
$userId = posix_getuid();
var_dump(posix_getpwuid($userId));

/*
array (size=7)
  'name' => string 'nginx' (length=5)
  'passwd' => string 'x' (length=1)
  'uid' => int 996
  'gid' => int 994
  'gecos' => string 'Nginx web server' (length=16)
  'dir' => string '/var/lib/nginx' (length=14)
  'shell' => string '/sbin/nologin' (length=13)
*/
```
