# PHP OpenSSL

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

## Encrypt, decrypt with bin2hex, hex2bin

```php
// Sender
echo '== ENCODE: ' . PHP_EOL;

$message = 'Hello world';
$algo = 'AES-256-CBC';
$secretKey = 'password';
$ivLength = openssl_cipher_iv_length($algo);
$iv = openssl_random_pseudo_bytes($ivLength);

$ciphertext = openssl_encrypt($message, $algo, $secretKey, 0, $iv);
echo 'ciphertext: ' . $ciphertext . PHP_EOL; // ciphertext: sBZFqL6h7l3oxAox/aN6Bg==

// IMPORTANT! We concatenate 2 values here
$messageEncoded = bin2hex($iv . $ciphertext);
echo 'bin2hex: ' . $messageEncoded . PHP_EOL; // bin2hex: c789e1d3b1ca6a887a834ff0fe0f57cf73425a46714c3668376c336f78416f782f614e3642673d3d

// Receiver
echo '== DECODE: ' . PHP_EOL;

$algo = 'AES-256-CBC';
$secretKey = 'password';
$ivLength = openssl_cipher_iv_length($algo);

$messageBin = hex2bin($messageEncoded);

$iv = substr($messageBin, 0, $ivLength);
if (strlen($iv) < $ivLength) {
    throw new Exception('Key length is less than ' . $ivLength);
}

$ciphertext = substr($messageBin, $ivLength);
echo 'ciphertext: ' . $ciphertext . PHP_EOL; // ciphertext: sBZFqL6h7l3oxAox/aN6Bg==

$message = openssl_decrypt($ciphertext, $algo, $secretKey, 0, $iv);
echo 'Decoded message: ' . $message . PHP_EOL; // Decoded message: Hello world
```

## Encrypt, decrypt with base64_encode, base64_decode and hash_hmac

```php
$message = "Hello world";
$cipher = "AES-128-CBC";
$secretKey = 'mypassword';
$ivLength = openssl_cipher_iv_length($cipher); 
$iv = openssl_random_pseudo_bytes($ivLength); // cipher initialization vector (iv)

// Encrypt
$ciphertextBin = openssl_encrypt($message, $cipher, $secretKey, OPENSSL_RAW_DATA, $iv);
$hmac = hash_hmac('sha256', $ciphertextBin, $secretKey, true);

// IMPORTANT! We concatenate 3 values here
$messageEncoded = base64_encode($iv . $hmac . $ciphertextBin);
echo "messageEncoded: " . $messageEncoded . "\n"; // messageEncoded: otDEV1YLD8HwB5SzInnl+kmeE/SHgSpGPtmDHj8gn3T5A6FSJgUKSnfnVybFNNtDxUAn6y4LnBwoIR8/kivVQg==

// Decrypt
$messageDecoded = base64_decode($messageEncoded);
$ivDecode = substr($messageDecoded, 0, $ivLength);
$hmacDecode = substr($messageDecoded, $ivLength, 32); // SHA256 is 32 bytes
$ciphertextBinDecoded = substr($messageDecoded, $ivLength + 32);
$messageDecrypted = openssl_decrypt($ciphertextBinDecoded, $cipher, $secretKey, OPENSSL_RAW_DATA, $ivDecode);
$hmacCalculated = hash_hmac('sha256', $ciphertextBinDecoded, $secretKey, true);

if (hash_equals($hmacDecode, $hmacCalculated)) {
    echo "Decrypted: " . $messageDecrypted . "\n"; // Decrypted: Hello world
} else {
    echo "Decryption failed: HMAC mismatch.\n";
}
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
