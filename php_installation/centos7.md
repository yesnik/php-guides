# PHP installation on Centos 7

- Run the following commands to enable both EPEL and Remi repositories:

```
sudo yum install epel-release yum-utils
sudo yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

- Enable the PHP 7.3 Remi repository:

```
sudo yum-config-manager --enable remi-php73
```

- Install PHP 7.3 and some of the most common PHP modules:

```
sudo yum install php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysqlnd
```

- Verify the PHP installation:

```
php -v
```

## Install PHP module

- Find available modules for `SOAP`:

```
yum search php-soap
```

- After this we can copy name of found module and install it:

```
yum install php73-php-soap.x86_64
```

- Check if module was installed

```
php -m
```
