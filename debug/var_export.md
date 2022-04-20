# PHP var_export

Short array syntax, indentation - 4 spaces

```php
function var_export_pretty($expression, $return = false)
{
    $export = var_export($expression, true);
    $export = preg_replace("/^([ ]*)(.*)/m", '$1$1$2', $export);
    $array = preg_split("/\r\n|\n|\r/", $export);
    $array = preg_replace(["/\s*array\s\($/", "/\)(,)?$/", "/\s=>\s$/"], [NULL, ']$1', ' => ['], $array);
    $export = join(PHP_EOL, array_filter(["["] + $array));
    if ($return) {
        return $export;
    } else {
        echo $export;
    } 
}
```
[Source](https://gist.github.com/Bogdaan/ffa287f77568fcbb4cffa0082e954022) 
