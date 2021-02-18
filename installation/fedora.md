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
- Add REMI Repository. *Note:* Check your Fedora's version: `cat /etc/os-release` 
  ```bash
  # Fedora 33
  sudo dnf install https://rpms.remirepo.net/fedora/remi-release-33.rpm
  
  # Fedora 32
  sudo dnf install https://rpms.remirepo.net/fedora/remi-release-32.rpm
  ```
- Enable repos
  ```bash
  sudo dnf config-manager --set-enabled remi-php80
  ```
- Install PHP 8 
  ```bash
  sudo dnf module reset php
  sudo dnf module install php:remi-8.0
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
 
