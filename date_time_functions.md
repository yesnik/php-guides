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
