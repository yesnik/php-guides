# Install PHP 7 on Ubuntu 18

* Update server:

```
sudo apt update
sudo apt upgrade
```

* Install PHP

```
sudo apt install php
```

Check installed version: `php -v`

## Install PHP modules

These are the most common PHP 7 modules often used by PHP applications:

```
sudo apt install php-pear php-fpm php-dev php-zip php-curl php-xmlrpc php-gd php-mysql php-mbstring php-xml php-apcu libapache2-mod-php
```

### Manual activation of modules

You don't need to edit `php.ini` to activate module. 
All modules' available config files are stored in `/etc/php/7.2/mods-available/` directory.

To activate module just add symlink:

```
sudo ln -s /etc/php/7.2/mods-available/apcu.ini /etc/php/7.2/cli/conf.d/20-apcu.ini
sudo ln -s /etc/php/7.2/mods-available/apcu.ini /etc/php/7.2/fpm/conf.d/20-apcu.ini
```

## Check all available PHP modules in Ubuntu

```
apt-cache search --names-only ^php
```

