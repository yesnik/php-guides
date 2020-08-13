# PHP on Fedora

## Update PHP

```bash
sudo dnf update php-cli
```

## Install PHP

### New Fedora version

New version of Fedora already has links to new PHP versions.

```bash
sudo dnf install php
```

### Old Fedora versions

- Update system
  ```bash
  sudo dnf -y update
  ```
- Add REMI Repository
  ```bash
  # Fedora 32
  sudo dnf -y install https://rpms.remirepo.net/fedora/remi-release-32.rpm

  # Fedora 31
  sudo dnf -y install https://rpms.remirepo.net/fedora/remi-release-31.rpm

  # Fedora 30
  sudo dnf -y install https://rpms.remirepo.net/fedora/remi-release-30.rpm
  ```
- Enable repos
  ```bash
  sudo dnf config-manager --set-enabled remi
  sudo dnf module reset php
  ```
- Install PHP 7.4
  ```bash
  sudo dnf module install php:remi-7.4
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
