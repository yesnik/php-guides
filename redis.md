# Redis with PHP

## Installation in Dockerfile

```Dockerfile
# PECL Packages
RUN pecl install -o -f redis && \
    pecl install igbinary \
      apcu \
      memcached

# Install PHP extensions
RUN docker-php-ext-enable \
        redis \
        igbinary \
        apcu \
        memcached
```

## Example

Get key `name` from Redis:

```php
$redis = new Redis([
    'host' => 'redis',
    'port' => 6379,
    'auth' => ['123123'],
]);

echo $redis->get('name');
```
