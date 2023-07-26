# String assignment

```php
$text = 'Hello';
$text[8] = 'World';
var_dump($text); // ?
echo strlen($text); // ?
```
<details><summary>Answer</summary>
<pre>
var_dump($text); // "Hello   W", Warning: Only the first byte will be assigned to the string offset
echo strlen($text); // 9
</pre>
</details>
