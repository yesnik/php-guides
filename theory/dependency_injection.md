# Dependency Injection in PHP

## Without DI

```php
class GoogleMaps
{
    public function getCityCoordinates($cityName) {
        // Get coordinates from GoogleMaps API
        return 'Google: ' . $cityName;
    }
}

class YandexMaps
{
    public function getCityCoordinates($cityName) {
        // Get coordinates from YandexMaps API
        return 'Yandex: ' . $cityName;
    }
}

class StoreService
{
    public function getCoordinates($cityName) {
        // We need manually edit this class to change the type of geolocation service
        
        // $geolocationService = new GoogleMaps();
        $geolocationService = new YandexMaps();
        
        return $geolocationService->getCityCoordinates($cityName);
    }
}

$service = new StoreService();

echo $service->getCoordinates('Moscow'); // Yandex: Moscow
```

**Note:** Without dependency injection, our classes are tightly coupled to their dependencies.

## With DI

```php
interface GeolocationInterface {
    public function getCityCoordinates($cityName);
}

class GoogleMaps implements GeolocationInterface
{
    public function getCityCoordinates($cityName) {
        // Get coordinates from Google API
        return 'Google: ' . $cityName;
    }
}

class YandexMaps implements GeolocationInterface
{
    public function getCityCoordinates($cityName) {
        // Get coordinates from YandexMaps API
        return 'Yandex: ' . $cityName;
    }
}

class StoreService
{
    public function __construct(GeolocationInterface $geolocaionService)
    {
        $this->geolocationService = $geolocaionService;
    }
    
    public function getCoordinates($cityName) {
        return $this->geolocationService->getCityCoordinates($cityName);
    }
}

// Now, it is for the user of the StoreService to decide which implementation to use. 
// And it can be changed anytime, without having to rewrite the StoreService.

// $geolocationService = new GoogleMaps();
$geolocationService = new YandexMaps();

// Pass dependency in the constructor
$service = new StoreService($geolocationService);

echo $service->getCoordinates('Moscow'); // Yandex: Moscow
```

You may see that dependency injection introduced one drawback: we have to *inject dependencies ourselves*:

```php
$geolocationService = new YandexMaps();
$service = new StoreService($geolocationService);
```

*Container* can solve this issue.

## Useful links

- [Understanding Dependency Injection](http://php-di.org/doc/understanding-di.html)
