# XDebug Installation

## Docker

### PHP 7.1

```
FROM php:7.1.26-fpm-alpine

RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-2.9.8 \
    && docker-php-ext-enable xdebug
```
