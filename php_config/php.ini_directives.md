# php.ini directives

## PHP_INI_* modes

See [docs](https://www.php.net/manual/en/configuration.changes.modes.php). These modes determine *when and where* a PHP directive may or may not be set. 
For example, some settings may be set within a PHP script using `ini_set()`, whereas others may require `php.ini` or `httpd.conf`.

- `PHP_INI_USER` - Entry can be set in user scripts (like with `ini_set()`) or in the Windows registry. Entry can be set in `.user.ini`
- `PHP_INI_PERDIR` - Entry can be set in `php.ini`, `.htaccess`, `httpd.conf` or `.user.ini`
- `PHP_INI_SYSTEM` - Entry can be set in `php.ini` or `httpd.conf`
- `PHP_INI_ALL` - Entry can be set anywhere

## Directives

### Check directive's value

```bash
php -i | grep upload_max_filesize
# upload_max_filesize => 2M => 2M
```

### memory_limit

Default: '128M', changeable: PHP_INI_ALL

This sets the maximum amount of memory in bytes that a script is allowed to allocate. 
This helps prevent poorly written scripts for eating up all available memory on a server. 
Note that to have no memory limit, set this directive to -1.

```
memory_limit = 128M
```

### post_max_size

Default: '8M', changeable: PHP_INI_PERDIR

Sets max size of post data allowed. This setting also affects file upload. 
To upload large files, this value must be larger than `upload_max_filesize`. 
Note: `memory_limit` should be larger than `post_max_size`. When an int is used, the value is measured in bytes. 

```
post_max_size = 20M
```

### upload_max_filesize

Default: '2M', changeable: PHP_INI_PERDIR

The maximum size of an uploaded file. `post_max_size` must be larger than this value.

```
upload_max_filesize = 10M
```
