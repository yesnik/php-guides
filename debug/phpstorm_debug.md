# PHPStorm Debug config

## Ubuntu 18.04

1. Install PHP extension:

```
sudo apt install php-xdebug
```

This command will create these symlinks:

- `/etc/php/7.2/cli/conf.d/20-xdebug.ini` -> `/etc/php/7.2/mods-available/xdebug.ini`
- `/etc/php/7.2/fpm/conf.d/20-xdebug.ini` -> `/etc/php/7.2/mods-available/xdebug.ini`

The contents of `/etc/php/7.2/mods-available/xdebug.ini`:

```
zend_extension=xdebug.so
xdebug.remote_enable=1
xdebug.remote_port = 9009
```

Check if `xdebug` module was installed:

```
php -m

[PHP Modules]
apc
apcu
...
xdebug
```

2. In PhpStorm open settings (`Ctrl+Alt+S`) and visit  *Languages & Frameworks - PHP - Debug*. Set `Debug port` to `9009`.

3. Install XDebug extension for your browser:

- Google Chrome - [XDebug helper](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc)
- Mozilla - [XDebug helper](https://addons.mozilla.org/en-US/firefox/addon/xdebug-helper-for-firefox/)

## Centos 7.4

1. Edit file `/etc/php.d/xdebug.ini`

```
[xdebug]
zend_extension=/usr/lib64/php/modules/xdebug.so
xdebug.remote_enable=1

; NOTE: This is IP of your PC in local network.
; It may not be remote address in $_SERVER['REMOTE_ADDR']
xdebug.remote_host=172.22.10.170
xdebug.remote_port=9009
xdebug.remote_log="/tmp/debug.log"
```

*Note*: assign to `remote_host` the IP of your PC

2. In PhpStorm open settings: *Languages & Frameworks - PHP - Debug* and press *Validate debugger configuration on the Web Server*.

If you get error `503` check PhpStorm's Proxy settings (Appearance & Behaviour - System Settings - HTTP Proxy). 
We have in this setting value `No proxy`.

3. Install XDebug extension for your browser.

