# JSON in PHP

## json_encode

Returns the JSON representation of a value. See [documentation](https://www.php.net/manual/en/function.json-encode.php)

```php
$arr = ['a' => 11, 'b' => 22];
echo json_encode($arr); // {"a":11,"b":22}
```

**Display non English letters**

```php
$arr = ['name' => 'Петя'];
echo json_encode($arr); // {"name":"\u041f\u0435\u0442\u044f"}
echo json_encode($arr, JSON_UNESCAPED_UNICODE); // {"name":"Петя"}
```

### Examples of json_encode options

```php
$a = ['<hi>',"'hello'",'"hey"','&ping&', "привет"];

echo "Usual: ", json_encode($a), "\n";
// Usual: ["<hi>","'hello'","\"hey\"","&ping&","\u043f\u0440\u0438\u0432\u0435\u0442"]

echo "JSON_HEX_TAG: ", json_encode($a, JSON_HEX_TAG), "\n";
// JSON_HEX_TAG: ["\u003Chi\u003E","'hello'","\"hey\"","&ping&","\u043f\u0440\u0438\u0432\u0435\u0442"]

echo "JSON_HEX_APOS: ", json_encode($a, JSON_HEX_APOS), "\n";
// JSON_HEX_APOS: ["<hi>","\u0027hello\u0027","\"hey\"","&ping&","\u043f\u0440\u0438\u0432\u0435\u0442"]

echo "JSON_HEX_QUOT: ", json_encode($a, JSON_HEX_QUOT), "\n";
// JSON_HEX_QUOT: ["<hi>","'hello'","\u0022hey\u0022","&ping&","\u043f\u0440\u0438\u0432\u0435\u0442"]

echo "JSON_HEX_AMP: ", json_encode($a, JSON_HEX_AMP), "\n";
// JSON_HEX_AMP: ["<hi>","'hello'","\"hey\"","\u0026ping\u0026","\u043f\u0440\u0438\u0432\u0435\u0442"]

echo "JSON_UNESCAPED_UNICODE: ", json_encode($a, JSON_UNESCAPED_UNICODE), "\n";
// JSON_UNESCAPED_UNICODE: ["<hi>","'hello'","\"hey\"","&ping&","привет"]

echo "ALL: ", json_encode($a, JSON_HEX_TAG | JSON_HEX_APOS | JSON_HEX_QUOT | JSON_HEX_AMP | JSON_UNESCAPED_UNICODE), "\n\n";
// ALL: ["\u003Chi\u003E","\u0027hello\u0027","\u0022hey\u0022","\u0026ping\u0026","привет"]

$b = [];

echo "Empty array as array: ", json_encode($b), "\n";
// Empty array as array: []

echo "Empty array as object: ", json_encode($b, JSON_FORCE_OBJECT), "\n\n";
// Empty array as object: {}

$c = [[1,2,3]];

echo "Display array as array: ", json_encode($c), "\n";
// Display array as array: [[1,2,3]]

echo "Display array as object: ", json_encode($c, JSON_FORCE_OBJECT), "\n\n";
// Display array as object: {"0":{"0":1,"1":2,"2":3}}

$d = ['name' => 'Kenny', 'age' => '18'];

echo "Associative array always displayed as object: ", json_encode($d), "\n";
// Associative array always displayed as object: {"name":"Kenny","age":"18"}

echo "Associative array always displayed as object: ", json_encode($d, JSON_FORCE_OBJECT), "\n\n";
// Associative array always displayed as object: {"name":"Kenny","age":"18"}
```
