# PHP on Fedora

## Update PHP

```bash
sudo dnf update php-cli
```

## Install PHP

### New Fedora version

You can use [Remi's config Wizard](https://rpms.remirepo.net/wizard/).

New version of Fedora already has links to new PHP versions.

```bash
sudo dnf install php
```

### Old Fedora versions

- Update system
  ```bash
  sudo dnf -y update
  ```
- Add REMI Repository. *Notes:* 
  - Check Fedora's version: `cat /etc/os-release`
  - See available [release versions](https://rpms.remirepo.net/fedora/)
  ```bash
  # Fedora 36
  sudo dnf install https://rpms.remirepo.net/fedora/remi-release-36.rpm
  
  # Fedora 37
  sudo dnf install https://rpms.remirepo.net/fedora/remi-release-37.rpm
  ```
- Enable repos
  ```bash
  sudo dnf config-manager --set-enabled remi-php80
  ```
- Install PHP 8 
  ```bash
  sudo dnf module reset php
  sudo dnf module install php:remi-8.2
  ```
- Check installed PHP version
  ```bash
  php -v
  ```

### Install popular PHP modules

 ```bash
sudo dnf install php-fpm php-mysqlnd php-zip \
    php-devel php-gd php-mcrypt php-mbstring \
    php-curl php-xml php-pear php-bcmath php-json php-intl php-opcache
 ```
 
 Other packages:
 
 - `php-pgsql` - adds `pdo_pgsql` module
 - `php-amqp`
 - `php-pecl-rdkafka` - [Kafka Client](https://github.com/arnaud-lb/php-rdkafka) for PHP
