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

## Fedora

1. Install `xdebug` extension:

```bash
yum install php-pecl-xdebug
```

## Docker-compose

### Config PHP container

Create file `docker/dev/php/conf.d/xdebug.ini`:

```
[xdebug]
xdebug.mode=debug
xdebug.client_host=host.docker.internal
xdebug.client_port=9003
```

Here `host.docker.internal` is an alias for your local PC IP address. In linux we need to modify `/etc/hosts` in our container:

```
172.27.0.1	host.docker.internal
```

We can do this automatically with script `entrypoint.sh`

```
#!/bin/sh
set -e

HOST_DOMAIN="host.docker.internal"
if ! ping -q -c1 $HOST_DOMAIN > /dev/null 2>&1
then
  HOST_IP=$(ip route | awk 'NR==1 {print $3}')
  # shellcheck disable=SC2039
  echo -e "$HOST_IP\t$HOST_DOMAIN" >> /etc/hosts
fi

# first arg is `-f` or `--some-option`
if [ "${1#-}" != "$1" ]; then
	set -- php-fpm "$@"
fi

exec "$@"
```

Copy this script in your `Dockerfile` for PHP-FPM:

```
FROM php:8-fpm-alpine

RUN apk update && apk add autoconf g++ make \
    && pecl install xdebug \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable xdebug

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

COPY ./dev/php/conf.d /usr/local/etc/php/conf.d

WORKDIR /app

COPY ./dev/php/entrypoint.sh /usr/local/bin/docker-php-entrypoint
RUN chmod +x /usr/local/bin/docker-php-entrypoint
```

### Config PHPStorm

- Open Settings > Languages & Frameworks > PHP > Servers.
- Add server with name `API`:
  - Host: 127.0.0.1
  - Port: 8080
  - Activate checkbox "Use path mappings" and define map: your local folder -> folder in docker
