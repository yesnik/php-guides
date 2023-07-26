# Logical operators

## Precedence of operator `and`

```php
$bool = true && false;
var_dump($bool); // false, that's expected

$bool = true and false;
var_dump($bool); // ?
```
<details><summary>Answer</summary>
It'll return <code>true</code>, because <code>and / or</code> have lower <a href="https://www.php.net/manual/en/language.operators.precedence.php">precedence</a> 
  than <code>=</code> but <code>|| / &&</code> have higher.
</details>
