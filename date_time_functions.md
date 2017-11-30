# Date and time functions

## Current time

```php
echo date('Y-m-d H:i:s'); // Returns string: '2017-11-03 12:00:00'
```

## Time from now

### 60 minutes ago
```php
echo date('Y-m-d H:i:s', strtotime("-60 minutes")); // Returns string: '2017-11-03 11:00:00'
```

### 3 hours ago
```php
echo date('Y-m-d H:i:s', strtotime('-3 hours')); // Returns string: '2017-11-03 09:00:00'
```

## Check if date is in interval

```php
$date = strtotime(date('Y-m-d'));
$startDate = strtotime( date('Y-m-d', strtotime('-1 day')) );
$endDate = strtotime( date('Y-m-d', strtotime('+10 day')) );

var_dump($startDate <= $date && $date <= $endDate); // true
```

## Get age by date of birhth

```php
/**
 * Method returns age by date of birth
 *
 * @param string $birthdate - date in format '2017-01-25'
 * @return null|string
 */
function getAge($birthdate)
{
    $datetime = date_create(date($birthdate));
    $datetimeCurrent = date_create(date('Y-m-d'));

    if ($datetime >= $datetimeCurrent) {
        return null;
    }

    $interval  = date_diff($datetime, $datetimeCurrent);

    return $interval->format('%y');
}
```
