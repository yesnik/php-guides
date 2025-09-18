# Password Hashing

See [documentation](https://www.php.net/manual/en/book.password.php)

## password_hash

Creates a password hash using defined algorithms.

```php
$password = '123';

// Generate hash of password. We can save it to Database
$hash = password_hash($password, PASSWORD_DEFAULT);
echo $hash;
/*
Output will be different each time:
'$2y$12$4Ik02B5dkGyTDTrk8m3iSe1F.vCmEcL5TGLmqRnOhfjeg7KUUvNq2'
'$2y$12$K6GrmGXy4fQVddpwaFaPaudREZDOwFv4jPyXLYTlO6oo4Iw6exrb2'
'$2y$12$.l0nnZbpTX7mcgWzwfJN7eMzy.aBjoDprP7ejYP2dSea5RruD8jGu'
*/
```

It is recommended to store the generated hash in a database column that can expand beyond 
60 characters (255 characters would be a good choice).

## password_verify

Returns true if a password matches a hash.

```php
$password = '123';

// Check if passed password matches the hash that we got from the database
if (password_verify($password, $hash)) {
  echo 'OK';
}
```
