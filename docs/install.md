JJSoft - AsgardCMS 
---
## TODO - HOW TO

## Install

### Get AsgardCMS

``` bash
$ composer create-project asgardcms/platform project-name
```
### Create a default database

crear la base primaria, la que maneja el acl, blog y frontend

``` mysql
mysql> create database bs_jjsoft;
```

### Install plattform
``` bash
$ php artisan asgard:install
```

### Config secondary database
Actualizar el archivo `config/database.php` dentro del array connections
```php
	'connections' => [
...
		'bs_siges' => [
            'driver'    => 'mysql',
            'host'      => env('BS_SIGES_HOST', 'localhost'),
            'database'  => env('BS_SIGES_DATABASE', 'forge'),
            'username'  => env('BS_SIGES_USERNAME', 'forge'),
            'password'  => env('BS_SIGES_PASSWORD', ''),
            'charset'   => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix'    => '',
            'strict'    => false,
        ],

...
	],
```

Luego actualizar el `.env` agregando las siguientes lineas
``` bash
BS_SIGES_HOST=localhost
BS_SIGES_DATABASE=bs_siges
BS_SIGES_USERNAME=root
BS_SIGES_PASSWORD=root
```
## Install Notification-module

```bash
$ composer require asgardcms/notification-module
$ php artisan module:publish notification
$ php artisan module:migrate notificacion
```

Si surge el error de memoria swap

```bash
$ sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
$ sudo /sbin/mkswap /var/swap.1
$ sudo /sbin/swapon /var/swap.1
```

