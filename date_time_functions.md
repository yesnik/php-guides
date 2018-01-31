# Date and time functions

## Current time

### Current time in server's timezone

```php
echo date('Y-m-d H:i:s'); // Returns string: '2017-11-03 12:00:00'

$dt = new DateTime();
echo $dt->format('Y-m-d H:i:s'); // Returns string: '2018-01-31 12:15:00'
```

### Current time in defined timezone

```php
$dt = new DateTime();
$dt->setTimezone(new DateTimeZone('UTC'));
echo $dt->format('Y-m-d H:i:s'); // Returns string: '2018-01-31 07:15:00'

$dt->setTimezone(new DateTimeZone('Europe/Moscow'));
echo $dt->format('Y-m-d H:i:s'); // Returns string: '2018-01-31 10:15:00'
```

List of timezones: [Asia](http://php.net/manual/en/timezones.asia.php), [Europe](http://php.net/manual/en/timezones.europe.php)


## Time with timezone

```php
$dt = new DateTime('2018-01-31T11:55:00Z');
echo $dt->format('Y-m-d H:i:s'); // 2018-01-31 11:55:00

$dt->setTimezone(new DateTimeZone('Asia/Yekaterinburg'));
echo $dt->format('Y-m-d H:i:s'); // 2018-01-31 16:55:00
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

### In 5 minutes

```php
$date = new DateTime();
$date->modify('+5 minutes');
echo $date->format('Y-m-d H:i:s'); // Returns string: '2018-01-29 05:08:56'
```

### Another examples

```php
echo date('Y-m-d H:i:s', strtotime('+5 minutes')); // 2018-01-29 06:05:00

echo date('Y-m-d H:i:s', strtotime('+2 hours')); // 2018-01-29 06:00:00

echo date('Y-m-d H:i:s', strtotime('+2 days')); // 2018-01-31 06:00:00

echo date('Y-m-d H:i:s', strtotime('+1 week')); // 2018-02-05 06:00:00

echo date('Y-m-d H:i:s', strtotime('+1 month')); // 2018-03-01 06:00:00

echo date('Y-m-d H:i:s', strtotime('+1 year')); // 2019-01-29 06:00:00
```

## Check if date is in interval

```php
$date = strtotime(date('Y-m-d'));
$startDate = strtotime( date('Y-m-d', strtotime('-1 day')) );
$endDate = strtotime( date('Y-m-d', strtotime('+10 day')) );

var_dump($startDate <= $date && $date <= $endDate); // true
```

## Get age by date of birth

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
