# SPL Functions

See [docs](https://www.php.net/manual/en/ref.spl.php)

### spl_object_hash

This function returns a unique identifier for the object. 
This id can be used as a hash key for storing objects, or for identifying an object, 
as long as the object is not destroyed. 
Once the object is destroyed, its hash may be reused for other objects.

### spl_object_id

This function returns a unique identifier for the object. 
The object id is unique for the lifetime of the object. Once the object is destroyed, its id may be reused for other objects.

```php
$a = new stdClass();
$b = new stdClass();
echo spl_object_hash($a) . "\n"; // '00000000000000010000000000000000'
echo spl_object_hash($b); // '00000000000000020000000000000000'
```
