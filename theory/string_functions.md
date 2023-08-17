# PHP String functions

## chr

```php
echo chr(65); // A
echo chr(66); // B
```

## htmlspecialchars

Converts special characters to HTML entities. It helps to avoid XSS issues.

```php
$name = $_GET['name'] ?? 'Guest';

echo "<h1>Hello, " . htmlspecialchars($name) . "</h1>";
// <h1>Hello, &lt;script&gt;alert(1)&lt;/script&gt;</h1>
```

## mb_str_split

[mb_str_split](https://www.php.net/manual/en/function.mb-str-split.php) — Given a multibyte string, return an array of its characters.

```php
$word = 'abc';

var_dump( mb_str_split($word) );
/*
array(3) {
  [0]=>
  string(1) "a"
  [1]=>
  string(1) "b"
  [2]=>
  string(1) "c"
}
*/
```

## ord

```php
echo ord('A'); // 65
echo ord('B'); // 66
```

## preg_split

```php
$string = ",12, A , ,  0, ";

$arr = preg_split("/\s*\,\s*/", $string, -1, PREG_SPLIT_NO_EMPTY);  
print_r($arr); // [12, A, 0]
```

## strrev

```php
echo strrev('ABC'); // CBA
```

## strtok

[strtok](https://www.php.net/manual/en/function.strtok.php) - Tokenize string. We need to call this function in a loop to get all tokens.

```php
$string = "/posts?a=1&b=2";
echo strtok($string, '?'); // 'posts'
```

```php
$string = "aa:bb:cc;:dd:";
$tok = strtok($string, ":;");

while ($tok !== false) {
    echo "Word = $tok" . PHP_EOL;
    $tok = strtok(":;");
}
/*
Word = aa
Word = bb
Word = cc
Word = dd
*/
```

## str_repeat

```php
echo str_repeat('ab', 3); // 'ababab'
```

## str_starts_with

PHP 8

```php
var_dump( str_starts_with('Kate', 'K') ); // bool(true)
```

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
