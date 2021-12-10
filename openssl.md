# PHP OpenSSL

## Where PHP seach certificates

```php
 var_dump(openssl_get_cert_locations());
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
