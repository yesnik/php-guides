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

### Convert time from one timezone to another

**Method 1**

```php
$dateString = '2019-03-20T09:17:25.000';

$timesoneMoscow = new DateTimeZone('Europe/Moscow'); 
$date = DateTime::createFromFormat('Y-m-d\TH:i:s.u', $dateString, $timesoneMoscow);

$timezoneYekaterinburg = new DateTimeZone('Asia/Yekaterinburg'); 
$date->setTimeZone($timezoneYekaterinburg);

echo $date->format('Y-m-d H:i:s'); // 2019-03-20 11:17:25
```

**Note:** In case of error function [DateTime::createFromFormat](https://www.php.net/manual/en/datetime.createfromformat.php) will return `false`. Use function `DateTime::getLastErrors()` to get the cause of error:

```php
$date = DateTime::createFromFormat('Y-m-d H:i:s', '2019-03-01 22:30');
var_dump($date); // bool(false)

print_r(DateTime::getLastErrors());
/*
Returns:

Array
(
    [warning_count] => 0
    [warnings] => Array
        (
        )

    [error_count] => 1
    [errors] => Array
        (
            [16] => Data missing
        )

)
*/
```

**Method 2**

```php
// Get current timezone
$currentTimezone = date_default_timezone_get(); // 'US/Pacific'

// Set timezone we want to convert time to
date_default_timezone_set('Europe/Moscow');

$dateTime = '2019-03-20T09:17:25.000';

$timestamp = strtotime($dateTime);

$dt = new DateTime();
$dt->setTimestamp($timestamp);
$dt->setTimezone(new DateTimeZone('Asia/Yekaterinburg'));

echo $dt->format('Y-m-d H:i:s'); // 2019-03-20 11:17:25

// Set initial settings of timezone
date_default_timezone_set($currentTimezone);
```

## Time from now

### date

```php
echo date('Y-m-d H:i:s', strtotime("-60 minutes")); // Returns string: '2017-11-03 11:00:00'

echo date('Y-m-d H:i:s', strtotime('-3 hours')); // Returns string: '2017-11-03 09:00:00'
```

### DateTime

**Using modify()**

```php
$date = new DateTime();
$date->modify('+5 minutes');
echo $date->format('Y-m-d H:i:s'); // Returns string: '2018-01-29 05:08:56'
```

**Using DateInterval**

```php
$date = new DateTime(); 
echo $date->format('Y-m-d H:i:s'); // Returns string: '2019-04-04 19:58:39'
// In 24 days
$date->add(new DateInterval('P24D'));
echo $date->format('Y-m-d H:i:s'); // Returns string: '2019-04-28 19:58:39'
```
```php
$date = new DateTime(); 
echo $date->format('Y-m-d H:i:s'); // Returns string: '2019-04-04 20:02:30'
// In 2 days 3 hours 30 minutes
$date->add(new DateInterval('P2DT3H30M'));
echo $date->format('Y-m-d H:i:s'); // Returns string: '2019-04-06 23:32:30'
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

## Difference between dates

### Difference between dates in seconds

```php
$datetime1 = strtotime('2018-02-05 00:00:00');
$datetime2 = strtotime('2018-02-05 00:01:30');

echo $datetime2 - $datetime1; // returns integer: 90
```

## Check if date is in interval

```php
$date = strtotime(date('Y-m-d'));
$startDate = strtotime( date('Y-m-d', strtotime('-1 day')) );
$endDate = strtotime( date('Y-m-d', strtotime('+10 day')) );

var_dump($startDate <= $date && $date <= $endDate); // true
```

## Round to nearest minute interval

```php
function roundToNearestMinuteInterval(\DateTime $dateTime, $minuteInterval = 10)
{
    return $dateTime->setTime(
        $dateTime->format('H'),
        round($dateTime->format('i') / $minuteInterval) * $minuteInterval,
        0
    );
}

$dt = new DateTime('2018-02-01 16:24:55');
$roundedDatetime = roundToNearestMinuteInterval($dt, 10);
echo $roundedDatetime->format('Y-m-d H:i:s'); // returns string '2018-02-01 16:20:00'

$dt = new DateTime('2018-02-01 16:29:00');
$roundedDatetime = roundToNearestMinuteInterval($dt, 10);
echo $roundedDatetime->format('Y-m-d H:i:s'); // returns string '2018-02-01 16:30:00'
```

Another useful functions about rounding time in PHP can be found at [stackoverflow](https://stackoverflow.com/a/40084666).

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
