# Vulcan Logic Dumper

See site: https://3v4l.org/

## Installation

See installation instructions at [Github](https://github.com/derickr/vld).

Create config file `/etc/php.d/10-vld.ini`:

```
; VLD config
extension=vld.so
```

After this you can get opcodes for your script:

```bash
php -dvld.active=1 main.php 
```
