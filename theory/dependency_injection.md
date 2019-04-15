# Dependency Injection in PHP

If you want to have a good well written application you should avoid dependencies between your modules/classes. There is a design pattern which could help and it's called *Dependency Injection (DI)*. It allows you to inject objects into a class, instead of creating them there.

*Dependency Injection Container* is the way to manage injecting and reading objects and third party libraries in your application.

PHP-FIG [PSR-11](https://www.php-fig.org/psr/psr-11/) is telling us how to have a container in you application:

```php
<?php
interface ContainerInterface {
    public function get($id);
    public function has($id);
}
```

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

## With PHP-DI

[PHP-DI](http://php-di.org/) is a dependency injection container.

It helps us to place injection of dependencies to config file.

1. Install *PHP-DI*: `composer require php-di/php-di`

2. Create files in `src/` directory:

```php
// File: src/GeolocationInterface.php
interface GeolocationInterface {
    public function getCityCoordinates($cityName);
}

// File: src/GoogleMaps.php
class GoogleMaps implements GeolocationInterface
{
    public function getCityCoordinates($cityName) {
        // Get coordinates from Google API
        return 'Google: ' . $cityName;
    }
}

// File: src/YandexMaps.php
class YandexMaps implements GeolocationInterface
{
    public function getCityCoordinates($cityName) {
        // Get coordinates from YandexMaps API
        return 'Yandex: ' . $cityName;
    }
}

// File: src/StoreService.php
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
```

3. Create `config.php`:

```php
return [
    'GeolocationInterface' => DI\autowire('GoogleMaps'),
];
```

4. Create `index.php`:

```php
ini_set('display_errors', 1);
error_reporting(E_ALL);

require __DIR__ . '/vendor/autoload.php';

$builder = new DI\ContainerBuilder();
$builder->addDefinitions('config.php');
$container = $builder->build();

$storeService = $container->get('StoreService');

echo $storeService->getCoordinates('Moscow'); // Google: Moscow
```

As you can see container helps us not to think about dependency injection in client code. In `config.php` we defined how dependencies should be resolved.

## Useful links

- [Understanding Dependency Injection](http://php-di.org/doc/understanding-di.html)
