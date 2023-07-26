# Increment `++`, decrement `--` operators

```php
$x = 3;
echo $x;
echo PHP_EOL;
echo $x+++$x++;
echo PHP_EOL;
echo $x;
echo PHP_EOL;
echo $x---$x--;
echo PHP_EOL;
echo $x;
```

<details><summary>Answer</summary>
<pre>
3
7
5
1
3
</pre>
<code>echo $x+++$x++;</code> is equivalent of <code>echo ($x++) + ($x++);</code>. This code increments variable twice.
</details>
