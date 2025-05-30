# PHP date and time functions

## ISO 8601 date format

```php
$dt = new DateTime();
echo $dt->format('c'); // Returns string: '2020-05-17T21:30:00-07:00'
```

## DateTimeImmutable

```php
echo (new \DateTimeImmutable('today'))->format('Y-m-d H:i:s'); // 2024-06-12 00:00:00
echo (new \DateTimeImmutable('January 1'))->format('Y-m-d H:i:s'); // 2024-01-01 00:00:00
echo (new \DateTimeImmutable('January 1 last year'))->format('Y-m-d H:i:s'); // 2023-01-01 00:00:00
echo (new \DateTimeImmutable('now next year'))->format('Y-m-d H:i:s'); // 2025-06-12 12:14:52
```

## Time with timezone

### Show time in another timezone

```php
$dt = new DateTime('2018-01-31T11:55:00Z');
echo $dt->format('Y-m-d H:i:s'); // 2018-01-31 11:55:00

$dt->setTimezone(new DateTimeZone('Asia/Yekaterinburg'));
echo $dt->format('Y-m-d H:i:s'); // 2018-01-31 16:55:00
```

### Current time in server's timezone

```php
echo date('Y-m-d H:i:s'); // Returns string: '2020-03-10 09:00:00'

$dt = new DateTimeImmutable();
echo $dt->format('Y-m-d H:i:s'); // Returns string: '2020-03-10 09:00:00'
```

### Current time in the defined timezone

```php
$dt = new DateTimeImmutable();
$dt->setTimezone(new DateTimeZone('UTC'));
echo $dt->format('Y-m-d H:i:s'); // Returns string: '2024-08-28 10:30:00'
```

```php
$dt = new DateTimeImmutable('2024-05-01T05:00:00');
$dtInMoscow = $dt->setTimezone(new DateTimeZone('Europe/Moscow'));

echo $dtInMoscow->format('Y-m-d H:i:s'); // Returns string: '2024-05-01 08:00:00'
```

```php
$timezoneMoscow = new DateTimeZone('Europe/Moscow'); 
$date = new DateTime('now', $timezoneMoscow);
echo $date->format('Y-m-d\TH:i:s.u'); // 2019-04-05T16:06:57.139769
```

List of timezones: [Asia](http://php.net/manual/en/timezones.asia.php), [Europe](http://php.net/manual/en/timezones.europe.php)

### Convert time from timestamp

```php
$dt = new DateTime();
$dt->setTimestamp(1582796891);
$dt->setTimezone(new DateTimeZone('Asia/Yekaterinburg'));
echo $dt->format('Y-m-d H:i:s'); // Returns string: '2020-02-27 14:48:11'
```

### Convert time from one timezone to another

**Method 1**

```php
$dateString = '2019-03-20T09:17:25.000';

$timezoneMoscow = new DateTimeZone('Europe/Moscow'); 
$date = DateTime::createFromFormat('Y-m-d\TH:i:s.u', $dateString, $timezoneMoscow);

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
echo date('Y-m-d H:i:s', strtotime("+15 minutes")); // Returns string: '2017-11-03 11:00:00'
echo date('Y-m-d H:i:s', strtotime('-3 hours')); // Returns string: '2017-11-03 09:00:00'

date('Y-m-d', strtotime('next Monday'));
date('Y-m-d', strtotime('last Thirsday'))
```

Available values: `week`, `month`, `year`

### DateTime

**Using constructor**

```php
// 3 days ago
$datetime = new \DateTime('-3 days');
var_dump($datetime);
/*
object(DateTime)#1 (3) {
  ["date"]=>
  string(26) "2020-08-14 04:59:24.153581"
  ["timezone_type"]=>
  int(3)
  ["timezone"]=>
  string(10) "US/Pacific"
}
*/
```
```php
echo (new DateTime('- 1 hour'))->format('Y-m-d H:i:s'); // 2022-01-25 03:47:03
```

**Using modify()**

```php
$date = new DateTime();
$date->modify('+5 minutes');
echo $date->format('Y-m-d H:i:s'); // Returns string: '2018-01-29 05:08:56'
```

**Using DateInterval**

[DateInterval](https://www.php.net/manual/en/dateinterval.construct.php) has parameter `$duration`. It's an interval specification. The format starts with the letter `P`, for period. Each duration period is represented by an integer value followed by a period designator. 
If the duration contains time elements, that portion of the specification is preceded by the letter `T`.

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

// 1 day ago
echo (new DateTime())->sub(new DateInterval('P1D'))->format('Y-m-d H:i:s');

// 1 day and 2 hours ago
// ([P]eriod [1 D]ay [T]ime [2 H]ours)
echo (new DateTime())->sub(new DateInterval('P1DT2H'))->format('Y-m-d H:i:s');
```

## Time from date

```php
$date = '2020-03-15';

echo date('Y-m-d', strtotime($date . ' - 5 days')); // Returns string: '2020-03-10'
```

## Common tasks with date and time

### Check if date is in interval

```php
$date = strtotime(date('Y-m-d'));
$startDate = strtotime( date('Y-m-d', strtotime('-1 day')) );
$endDate = strtotime( date('Y-m-d', strtotime('+10 day')) );

var_dump($startDate <= $date && $date <= $endDate); // true
```

### Difference between dates in seconds

```php
$datetime1 = strtotime('2018-02-05 00:00:00');
$datetime2 = strtotime('2018-02-05 00:01:30');

echo $datetime2 - $datetime1; // returns integer: 90
```

### Parse date to day, month, year

[strtotime()](https://www.php.net/manual/en/function.strtotime.php) - Parse about any English textual datetime description into a Unix timestamp.

```php
$birth_date = '24.11.1990';
$birth_date = '1990-11-26';
$birth_date = '11/24/1990';

if ($timestamp = strtotime($birth_date)) {
    echo date('d', $timestamp); // 24
    echo date('m', $timestamp); // 11
    echo date('Y', $timestamp); // 1990
} else {
	echo 'Invalid date';
}
```

### Round to nearest minute interval

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

## Convert date format

```php
$dateString = '21.01.1990';
// Method returns `false` in case of conversion error
$dateTime = DateTime::createFromFormat('d.m.Y', $dateString);

if ($dateTime !== false) {
    echo $dateTime->format('Y-m-d'); // '1990-01-21'
}
```

### Get age by date of birth

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

### Get dates between 2 dates

```php
$startDate = new DateTimeImmutable('2024-02-28');
$endDate = new DateTimeImmutable('2024-03-02');

$interval = new DateInterval('P1D');
$dateRange = new DatePeriod($startDate, $interval, $endDate->modify('+1 day'));

foreach($dateRange as $date){
    echo $date->format("Y-m-d") . "\n";
}
/*
2024-02-28
2024-02-29
2024-03-01
2024-03-02
*/

// Ensure that code hasn't modified args
echo $startDate->format('Y-m-d') . PHP_EOL; // 2024-02-28
echo $endDate->format('Y-m-d'); // 2024-03-02
```
