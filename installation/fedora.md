# Install PHP on Fedora

- Update system
  ```bash
  sudo dnf -y update
  ```
- Add REMI Repository
  ```bash
  # Fedora 31
  sudo dnf -y install https://rpms.remirepo.net/fedora/remi-release-31.rpm

  # Fedora 30
  sudo dnf -y install https://rpms.remirepo.net/fedora/remi-release-30.rpm

  # Fedora 29
  sudo dnf -y install https://rpms.remirepo.net/fedora/remi-release-29.rpm
  ```
- Enable repos
  ```bash
  sudo dnf config-manager --set-enabled remi
  sudo dnf config-manager --set-enabled remi-php74
  ```
- Install PHP 7.4
  ```bash
  sudo dnf module install php:remi-7.4
  ```
- Check installed PHP version
  ```bash
  php -v
  ```
- Install popular modules
   ```bash
   sudo dnf install php-fpm php-mysqlnd php-zip php-devel php-gd php-mcrypt php-mbstring php-curl php-xml php-pear php-bcmath php-json
   ```
