# Info about installed PHP

### Get PHP extension folder

```bash
php-config --extension-dir
```

If `php-config` doesn't exist, then `apt-get install php-config` if Ubuntu/Debian or `yum install php-config` if CentOS/Red Hat.

That command will give exact location of your php extension folder.

### Show all PHP config files

```bash
php --ini
```
This command will show loaded config file `/etc/php.ini` and additional `.ini` files in: `/etc/php.d`.

