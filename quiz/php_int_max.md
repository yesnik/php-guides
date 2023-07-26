# PHP_INT_MAX

`PHP_INT_MAX` (int) - The largest integer supported in this build of PHP. 

```php
var_dump(PHP_INT_MAX); // ?
var_dump(PHP_INT_MAX + 1); // ?
var_dump((int)(PHP_INT_MAX + 1)); // ?
var_dump((int)(PHP_INT_MAX + 2)); // ?
var_dump(
	(int)(PHP_INT_MAX + 1) === (int)(PHP_INT_MAX + 2)
); // ?
```

<details><summary>Answer</summary>
  
- `int(2147483647)` in 32 bit systems, `2^31 - 1`<br>
- `int(9223372036854775807)` in 64 bit systems, `2^63 - 1`<br>
- PHP handles large integers by <b>converting them to float</b> (which can store larger values).<br>
- result of <code>var_dump((int)(PHP_INT_MAX + 1))</code> will be displayed as a <b>negative number</b>.<br>

<pre>
var_dump(PHP_INT_MAX); // int(9223372036854775807)
var_dump(PHP_INT_MAX + 1); // float(9.223372036854776E+18)
var_dump((int)(PHP_INT_MAX + 1)); // int(-9223372036854775808)
var_dump((int)(PHP_INT_MAX + 2)); // int(-9223372036854775808)
var_dump(
	(int)(PHP_INT_MAX + 1) === (int)(PHP_INT_MAX + 2)
); // true
</pre>
</details>
