# Hash

A hashing algorithm is a method used to convert data into a fixed-size string of characters.

## Ideal cryptographic hash function

- It should be fast to compute the hash value for any kind of data;
- It should be impossible to regenerate a message from its hash value (brute force attack is the only option);
- It should be impossible to find two messages with the same hash (a collision);
- Every change to a message, even the smallest one, should change the hash value. It should be completely different. It’s called the avalanche effect.

## `hash()`

[hash](https://www.php.net/manual/en/function.hash.php) - Generate a hash value (message digest)

```php
echo hash('md5', 'Hello'); // 32 symbols:  "8b1a9953c4611296a827abf8c47804d7"
echo hash('sha1', 'Hello'); // 40 symbols: "f7ff9e8b7bb2e09b70935a5d785e0cc5d9d0abf0"
echo hash('sha256', 'Hello'); // 64 symbols: "185f8db32271fe25 ... 304eda64826381969"
echo hash('sha3-512', 'Hello'); // 128 symbols: "0b844ac991e ... 1afc4bd5a8e34627"
echo hash('whirlpool', 'Hello'); // 128 symbols: "00acca7f805 ... 0ed2fb56bf326bca"
```

## `hash_algos()`

[hash_algos](https://www.php.net/manual/en/function.hash-algos.php) - Return a list of registered hashing algorithms

`MD5`, once considered safe, is now completely compromised. 
Then there was `SHA-1`, which is now unsafe. 
The same thing will surely happen to the widely used `SHA-2` someday.

```php
print_r(hash_algos());

/*
(
    [0] => md2
    [1] => md4
    [2] => md5
    [3] => sha1
    [4] => sha224
    [5] => sha256
    [6] => sha384
    [7] => sha512/224
    [8] => sha512/256
    [9] => sha512
    [10] => sha3-224
    [11] => sha3-256
    [12] => sha3-384
    [13] => sha3-512
    [14] => ripemd128
    [15] => ripemd160
    [16] => ripemd256
    [17] => ripemd320
    [18] => whirlpool
    [19] => tiger128,3
    [20] => tiger160,3
    [21] => tiger192,3
    [22] => tiger128,4
    [23] => tiger160,4
    [24] => tiger192,4
    [25] => snefru
    [26] => snefru256
    [27] => gost
    [28] => gost-crypto
    [29] => adler32
    [30] => crc32
    [31] => crc32b
    [32] => crc32c
    [33] => fnv132
    [34] => fnv1a32
    [35] => fnv164
    [36] => fnv1a64
    [37] => joaat
    [38] => murmur3a
    [39] => murmur3c
    [40] => murmur3f
    [41] => xxh32
    [42] => xxh64
    [43] => xxh3
    [44] => xxh128
    [45] => haval128,3
    [46] => haval160,3
    [47] => haval192,3
    [48] => haval224,3
    [49] => haval256,3
    [50] => haval128,4
    [51] => haval160,4
    [52] => haval192,4
    [53] => haval224,4
    [54] => haval256,4
    [55] => haval128,5
    [56] => haval160,5
    [57] => haval192,5
    [58] => haval224,5
    [59] => haval256,5
)
*/
```
