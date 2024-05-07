# How to set timezone in PHP

Edit file: `sudo vim /etc/php.ini`:

```
date.timezone = Asia/Yekaterinburg
```

See [available timezones](http://php.net/manual/ru/timezones.php)

### Dockerfile

```Dockerfile
RUN echo '[PHP]\ndate.timezone = "Asia/Yekaterinburg"\n' > /usr/local/etc/php/conf.d/timezone.ini
```
