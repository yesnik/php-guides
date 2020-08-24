# Convert XML to PHP array

```php
$xml = <<<XML
<MT_GENERAL>
    <TASK>
        <PROCESS_TYPE>ZINT</PROCESS_TYPE>
        <LINK>Loan</LINK>
        <DATE_FROM>2020-08-05</DATE_FROM>
    </TASK>
</MT_GENERAL>
XML;

// Convert xml string into an object of class SimpleXMLElement
$obj = simplexml_load_string($xml);

// Convert to JSON
$json = json_encode($obj);

// Convert into associative array
$arr = json_decode($json, true);

var_export($arr);
/* Returns:
array (
  'TASK' =>
  array (
    'PROCESS_TYPE' => 'ZINT',
    'LINK' => 'Loan',
    'DATE_FROM' => '2020-08-05',
  ),
)
*/
```
