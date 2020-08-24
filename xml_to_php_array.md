# Convert XML to PHP array

## Using preg_replace

```php
$xml = <<<XML
  <MT_GENERAL xmlns="URN:CREATE_BP_AND_BO_LP13">
      <TASK xmlns="">
          <PROCESS_TYPE xmlns="">ZINV</PROCESS_TYPE>
          <LINK xmlns="">Loan</LINK>
          <DATE_FROM xmlns="">2020-08-24</DATE_FROM>
          <TIME_FROM xmlns="">13:08:04.0Z</TIME_FROM>
          <CATEGORY xmlns=""></CATEGORY>
          <QUEUE xmlns="">loans-general</QUEUE>
      </TASK>
  </MT_GENERAL>
XML;

$xml = preg_replace('/xmlns=".*"/', '', $xml);
$xml = preg_replace('/<\/.*$/m', "'", $xml);
$xml = str_replace(['<', '>'], "'", $xml);
$xml = preg_replace("/\'(.*)\s\'(.+)\'/", '\'$1\' => \'$2\',', $xml);
$xml = preg_replace('/\s*\'$/m', "\n\t],", $xml);
$xml = preg_replace("/\'(\w+)\s\'/m", '\'$1\' => [', $xml);

echo $xml;
/* Returns:

'MT_GENERAL' => [
  'TASK' => [
    'PROCESS_TYPE' => 'ZINV',
    'LINK' => 'Loan',
    'DATE_FROM' => '2020-08-24',
    'TIME_FROM' => '13:08:04.0Z',
    'CATEGORY' => [],
    'QUEUE' => 'loans-general',
  ],
],
*/
```
