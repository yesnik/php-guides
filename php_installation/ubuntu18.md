# PHP installation on Ubuntu 18

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
