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

## Example

We have file `main.php`:

```php
<?php
$a = 1;
echo $a + 4;

```

Run command:

```bash
php -dvld.active=1 hello.php
```

We'll see output in console:

```
Finding entry points
Branch analysis from position: 0
1 jumps found. (Code = 62) Position 1 = -2
filename:       /home/nik/php/main.php
function name:  (null)
number of ops:  4
compiled vars:  !0 = $a
line      #* E I O op                           fetch          ext  return  operands
-------------------------------------------------------------------------------------
    2     0  E >   ASSIGN                                                   !0, 1
    3     1        ADD                                              ~1      !0, 4
          2        ECHO                                                     ~1
    4     3      > RETURN                                                   1

branch: #  0; line:     2-    4; sop:     0; eop:     3; out0:  -2
path #1: 0, 5
```
