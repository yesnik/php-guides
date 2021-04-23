# Test traits

## Trait

```php
trait OfficeAppointableTrait
{
    public function needAppointment(Claims $claim): bool
    {
        return !empty($claim->info->getDataParam('visit_office_code'));
    }
}
```

## Test

```php
// ...
$object = $this->getObjectForTrait(OfficeAppointableTrait::class);

$this->assertTrue($object->needAppointment($claim));
```
