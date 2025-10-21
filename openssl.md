# PHP OpenSSL

- **HMAC** (Hash-based message authentication code) is a cryptographic authentication technique that uses a hash function and a secret key.
- Don't use `ECB` mode, use `CBC` instead.
- Never use plain text as encryption key. Always hash the plain text key and then use for encryption.
- Always use Random `IV` (Initialization Vector) for encryption and decryption. True randomization is important.
- You should always use a *random* Initialization Vector if you want to protect against replaying attacks (although the probability of it happening is low).
  You can just append or prepend the `IV` to the output and use it in your decrypt function, it may be exposed publically.
- The `IV` should be generated for each encrypted message and transmitted with the message. 
    Sending 2 or more messages with the same `IV` is a serious security flaw. 
    You can use `$iv = openssl_random_pseudo_bytes(16)` to generate `IV`. This shouldn't rely on insecure random number generators such as `rand()`.

## OpenSSL Functions 

### openssl_get_cert_locations()

It's where PHP seach certificates.

```php
var_dump(openssl_get_cert_locations());
/*
array(8) {
  ["default_cert_file"]=>
  string(21) "/etc/pki/tls/cert.pem"
  ["default_cert_file_env"]=>
  string(13) "SSL_CERT_FILE"
  ["default_cert_dir"]=>
  string(18) "/etc/pki/tls/certs"
  ["default_cert_dir_env"]=>
  string(12) "SSL_CERT_DIR"
  ["default_private_dir"]=>
  string(20) "/etc/pki/tls/private"
  ["default_default_cert_area"]=>
  string(12) "/etc/pki/tls"
  ["ini_cafile"]=>
  string(0) ""
  ["ini_capath"]=>
  string(0) ""
}
*/
```

### openssl_get_cipher_methods()

```php
$cipherMethods = openssl_get_cipher_methods();
print_r($cipherMethods);
/*
Array
(
    [0] => AES-128-CBC
    [1] => AES-128-CBC-HMAC-SHA1
    [2] => AES-128-CBC-HMAC-SHA256
    ...
*/
```

## Create public / private keys

```php
$config = [
  'digest_alg' => 'sha512',
  'private_key_bits' => 2048,
  'private_key_type' => OPENSSL_KEYTYPE_RSA,
];

// Generates a new private key
$privateKey = openssl_pkey_new($config);

// Gets an exportable representation of a key into a string
openssl_pkey_export($privateKey, $privateKeyPem);

file_put_contents('./keys/private.pem', $privateKeyPem);

// Returns an array with the key details
$publicKey = openssl_pkey_get_details($privateKey);
file_put_contents('./keys/public.pem', $publicKey['key']);
```
