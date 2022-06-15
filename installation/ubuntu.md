# PHP installation on Ubuntu

* Update server:

```
sudo apt update
sudo apt upgrade
```

* Install PHP

```bash
sudo apt install php
```

* Install popular modules

```bash
sudo apt install php-fpm php-json  \
    php-mysql php-pgsql \
    php-amqp php-zip  \
    php-gd php-mbstring php-curl php-xml php-pear php-bcmath  \
    php-intl php-xdebug 
```

Check installed version: `php -v`

## Import Ondřej Surý PHP PPA

To install new version of PHP we can import the good renowned PPA from [Ondřej Surý](https://github.com/oerdnj), the lead developer on PHP and Debian.

```
sudo apt install software-properties-common && sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
sudo apt upgrade -y
```

## PHP module installation

### Manual activation of modules

You don't need to edit `php.ini` to activate module. 
All modules' available config files are stored in `/etc/php/7.2/mods-available/` directory.

To activate module just add symlink:

```
sudo ln -s /etc/php/7.2/mods-available/apcu.ini /etc/php/7.2/cli/conf.d/20-apcu.ini
sudo ln -s /etc/php/7.2/mods-available/apcu.ini /etc/php/7.2/fpm/conf.d/20-apcu.ini
```

### Check all available PHP modules

```
apt-cache search --names-only ^php
```
