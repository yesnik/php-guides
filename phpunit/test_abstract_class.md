# Test Abstract Class

## Use `getMockForAbstractClass()`

We want to test method `getResidentialAddress` of abstract class:

```php
abstract class AbstractSoapStructureCreator
{
    abstract public function createStructure(): array;

    protected function getResidentialAddress(ClaimForm $model): array
    {
        // ...
    }
}
```

Write test:

```php
/** @dataProvider getResidentialAddressCases */
public function testGetResidentialAddress(array $formData, array $expectedStructure): void
{
    $object = self::getMockForAbstractClass(AbstractSoapStructureCreator::class, [$this->claimMock, $this->datetime]);

    $method = new ReflectionMethod($object, 'getResidentialAddress');
    $method->setAccessible(true);

    $model = new OpenBkiForm();
    $model->setAttributes($formData);

    self::assertEquals($expectedStructure, $method->invoke($object, $model));
}

public function getResidentialAddressCases(): array
{
    return [
        'We have register_place_code' => [
            'formData' => [
                'city_code' => '6300000100000',
                'live_place_code' => '6302700004800',
                'live_place_name' => 'Rostov',
                'live_street_name' => 'Lenin street',
                'live_home_number' => '20',
                'live_home_building' => '2',
                'live_home_apartment' => '220',
                'live_home_index' => '446733',
            ],
            'expectedStructure' => [
                'POST_CODE1' => '446733',
                'REGION' => '63',
                'REGIOGROUP' => '027',
                'CITY1' => 'Rostov',
                'CITY2' => '',
                'STREET' => 'Lenin street',
                'HOUSE_NUM1' => '20',
                'HOUSE_NUM2' => '2',
                'ROOMNUMBER' => '220',
            ],
        ],
    ]
}
```
