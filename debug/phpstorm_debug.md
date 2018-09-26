# PHPStorm Debug config

1. Edit file `/etc/php.d/xdebug.ini`

```
[xdebug]
zend_extension=/usr/lib64/php/modules/xdebug.so
xdebug.remote_enable=1
xdebug.remote_host=172.22.10.170
# xdebug.remote_connect_back=1
xdebug.remote_port=9009
xdebug.remote_log="/tmp/debug.log"
```

*Note*: assign to `remote_host` the IP of your PC

2. In PhpStorm open settings: *Languages & Frameworks - PHP - Debug* and press *Validate debugger configuration on the Web Server*.

If you get error `503` check PhpStorm's Proxy settings (Appearance & Behaviour - System Settings - HTTP Proxy). 
We have in this setting value `No proxy`.

3. Install XDebug extension for your browser:

- Google Chrome - [XDebug helper](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc)
