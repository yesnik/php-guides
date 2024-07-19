# PHP FPM

## Ubuntu

```
apt install php-fpm
```

Edit config `/etc/php/7.2/fpm/pool.d/www.conf`:

```
; listen = /run/php/php7.2-fpm.sock
listen = 9000
```

Restart php-fpm:

```
service php7.2-fpm restart
```
