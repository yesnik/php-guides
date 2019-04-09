# Dependency Injection in PHP

## Without DI

```php
class Store
{
    private $address;
    
    public function __construct($address)
    {
        $this->address = $address;
    }
    
    public function getAddress()
    {
        return $this->address;
    }
}

class GoogleMaps
{
    public function getCoordinatesFromAddress($address) {
        // Get coordinates from Google API
        return 'Google: ' . $address;
    }
}

class OpenStreetMap
{
    public function getCoordinatesFromAddress($address) {
        // Get coordinates from OpenStreet
        return 'OpenStreet: ' . $address;
    }
}

class StoreService
{
    public function getStoreCoordinates($store) {
        // We need manually edit this class to change type of geolocation service:
        // Without dependency injection, your classes are tightly coupled to their dependencies
        
        // $geolocationService = new GoogleMaps();
        $geolocationService = new OpenStreetMap();
        
        return $geolocationService->getCoordinatesFromAddress($store->getAddress());
    }
}

$moscowStore = new Store('Moscow');
$storeService = new StoreService();

echo $storeService->getStoreCoordinates($moscowStore); // OpenStreet: Moscow
```

## With DI

```php
class Store
{
    private $address;
    
    public function __construct($address)
    {
        $this->address = $address;
    }
    
    public function getAddress()
    {
        return $this->address;
    }
}

// The services are defined using an interface
interface GeolocationService {
    public function getCoordinatesFromAddress($address);
}

class GoogleMaps implements GeolocationService
{
    public function getCoordinatesFromAddress($address) {
        // Get coordinates from Google API
        return 'Google: ' . $address;
    }
}

class OpenStreetMap implements GeolocationService
{
    public function getCoordinatesFromAddress($address) {
        // Get coordinates from OpenStreet
        return 'OpenStreet: ' . $address;
    }
}

class StoreService
{
    private $geolocationService;
    
    public function __construct(GeolocationService $geolocationService)
    {
        $this->geolocationService = $geolocationService;
    }
    
    public function getStoreCoordinates($store) {
        return $this->geolocationService->getCoordinatesFromAddress($store->getAddress());
    }
}

$moscowStore = new Store('Moscow');

// Now, it is for the user of the StoreService to decide which implementation to use. 
// And it can be changed anytime, without having to rewrite the StoreService.

//$geolocationService = new GoogleMaps();
$geolocationService = new OpenStreetMap();

$storeService = new StoreService($geolocationService);

echo $storeService->getStoreCoordinates($moscowStore); // OpenStreet: Moscow
```

You may see that dependency injection will leave with one drawback: you now have to *handle injecting dependencies*:

```php
$geolocationService = new OpenStreetMap();
$storeService = new StoreService($geolocationService);
```

*Container* can solve this issue.

## Useful links

- [Understanding Dependency Injection](http://php-di.org/doc/understanding-di.html)
