# PHP String functions

## trim

```php
trim(" hey  \n"); // 'hey'
trim('__hey_world_', '_'); // 'hey_world'
trim('_,_,_hey_world_x', '_,x'); // 'hey_world'
```

## uniqid

[uniqid](https://www.php.net/manual/en/function.uniqid.php) gennerates prefixed unique identifier based on the current time in microseconds.

```php
uniqid(); // 5e203c4c794d0
uniqid('server1_'); // server1_5e203c806552f
uniqid('', true); // 5e203ca619bc02.47789457
```
