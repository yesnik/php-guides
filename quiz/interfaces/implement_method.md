# Interfaces

## What error will be thrown?

```php
interface OfficeAppointable
{
    public function needAppointment(): bool;
}

class Form implements OfficeAppointable
{
    public function needAppointment($type): bool
    {
        return $type !== 'delivery';
    }
}

$form = new Form();
var_dump($form->needAppointment('office'));
```
