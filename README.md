## Bitoid Docker4Drupal
# This is "Bitoid" dcoker4drupal installation instructions i have used for my project:

Docker4Drupal is an open-source project [GitHub page](https://github.com/wodby/docker4drupal) that provides pre-configured `docker-compose.yml` file with images to spin up local environment on Linux, Mac OS X and Windows.

## Requirements ##
* [Install Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).

> Note: The instructions below refer to the [global Composer installation](https://getcomposer.org/doc/00-intro.md#globally).
You might need to replace `composer` with `php composer.phar` (or similar) for your setup.

* Install Docker ([Linux](https://docs.docker.com/engine/install/), [Docker for Mac](https://docs.docker.com/desktop/mac/) or [Docker for Windows (10+ Pro)](https://docs.docker.com/desktop/windows/))

* For Linux additionally install [docker compose](https://docs.docker.com/compose/install/).

## Installation ##

1. Make direction file for your project.
```
mkdir drupal-project
```

2. Create composer project:
```
composer create-project drupal-composer/drupal-project:9.x-dev drupal-project --no-interaction
```

3. Download and unpack `docker4drupal.tar.gz` from the [latest stable release](https://github.com/wodby/docker4drupal/releases) to your project root.
* Go to project root.
```
cd drupal-project
```
* Unpack `docker4drupal.tar.gz`

4. Delete `docker-compose.override.yml`.

5. Create configuration file.
```
mkdir config/sync -p
```

6. User permisions for container.
```
chown -R 1000:1000 ./
```

7. Database acceess settings in your `.env` file:
```
MYSQL_DATABASE=${DB_NAME}
MYSQL_HOSTNAME=${DB_HOST}
MYSQL_PASSWORD=${DB_PASSWORD}
MYSQL_PORT=${DB_PORT}
MYSQL_USER=${DB_USER}
```

8. Uncomment `13` and `15` lines in `docker-compose.yml` to prevent erasing database after `docker-compose down` command.
9. Database driver in `settings.php`.
```
$databases['default']['default'] = array (
  'database' => 'drupal',
  'username' => 'drupal',
  'password' => 'drupal',
  'prefix' => '',
  'host' => 'mariadb',
  'port' => '3306',
  'namespace' => 'Drupal\mysql\Driver\Database\mysql',
  'driver' => 'mysql',
  'autoload' => 'core/modules/mysql/src/Driver/Database/mysql/',
);
```
### Congrats you can run Drupal!
```
docker-compose up -d
```
## Configure Your Environment for Theme Development. ##

1. To turn on Drupal errors add this line in `settings.php`.
```
$config['system.logging']['error_level'] = 'verbose';
```
2. 


---------------------------------------------------------------------------------------------------------------------------------------------------------
## Installing themes or plugins ##
```
docker-compose exec php composer require 'drupal/<theme or plugin name>'
```




