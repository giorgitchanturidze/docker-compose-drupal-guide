# Bitoid [Docker4Drupal](https://github.com/wodby/docker4drupal)
## This is "Bitoid" [Docker4Drupal](https://github.com/wodby/docker4drupal) installation instructions i have used for my project:

Docker4Drupal is an open-source project [GitHub page](https://github.com/wodby/docker4drupal) that provides pre-configured `docker-compose.yml` file with images to spin up local environment on Linux, Mac OS X and Windows.

## Requirements ##
* [Install Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).

> _**Note**: The instructions below refer to the [global Composer installation](https://getcomposer.org/doc/00-intro.md#globally).
You might need to replace `composer` with `php composer.phar` (or similar) for your setup._

* [Install Docker ubuntu](https://docs.docker.com/engine/install/ubuntu/).

* [Install Docker compose](https://docs.docker.com/compose/install/compose-plugin/#installing-compose-on-linux-systems)
* To run docker without `sudo` enter in terminal:
```
sudo groupadd docker
sudo usermod -aG docker $USER
```
Restart PC 

## Installation ##

**1.** Make direction file for your project.
```
mkdir drupal-project
```

**2.** Create composer project:
```
composer create-project drupal-composer/drupal-project:9.x-dev drupal-project --no-interaction
```

**3.** Download and unpack `docker4drupal.tar.gz` from the [latest stable release](https://github.com/wodby/docker4drupal/releases) to your project root.
* Go to project root.
```
cd drupal-project
```
* Unpack `docker4drupal.tar.gz`

**4.** Delete `docker-compose.override.yml`.

**5.** Create configuration file.
```
mkdir config/sync -p
```

**6.** User permisions for container.
```
chown -R 1000:1000 ./
```

**7.** Database acceess settings in your `.env` file:
```
MYSQL_DATABASE=${DB_NAME}
MYSQL_HOSTNAME=${DB_HOST}
MYSQL_PASSWORD=${DB_PASSWORD}
MYSQL_PORT=${DB_PORT}
MYSQL_USER=${DB_USER}
```
**8.** A common use case is to supply database credentials via the environment. Edit `settings.php`.
```
$databases['default']['default'] = [
   'database' => $_ENV['MYSQL_DATABASE'],
   'driver' => 'mysql',
   'host' => $_ENV['MYSQL_HOSTNAME'],
   'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
   'password' => $_ENV['MYSQL_PASSWORD'],
   'port' => $_ENV['MYSQL_PORT'],
   'prefix' => '',
   'username' => $_ENV['MYSQL_USER'],
 ];
```


**9.** Uncomment `13` and `15` lines in `docker-compose.yml` to prevent erasing database after `docker-compose down` command.

### Congrats you can now run Drupal!
* Run in terminal:
```
docker-compose up -d
```
* Open browser and go to this adress:
 ```
 http://drupal.docker.localhost:8000/
```
---------------------------------------------------------------------------------------------------------------------------------------------------------

## Configure Your Environment for Drupal Development. ##

## Enable developer/debug mode. ##
Uncommenting the following lines in the `settings.php` file. `emacs sites/default/settings.php`. Make sure this code is at the bottom of your `settings.php` file so that local settings can override default settings.
```
if (file_exists(__DIR__ . '/settings.local.php')) {
  include __DIR__ . '/settings.local.php';
}
```
and then copying the file `example.settings.local.php` from `/sites` folder to `/sites/default` folder and rename it to `settings.local.php`
```
cp sites/example.settings.local.php sites/default/settings.local.php
```
It adds a few settings which will help you in debugging and making development easier. If you don't want any of them in particular, you can always comment them out.
_Note : If you think adding a `file_exists` call to each page will slow down the site, you can always remove it in the production code._

 **Disable render caching and JavaScript/CSS aggregation**
* Uncomment lines in settings.local.php
1. Ensure that the following lines are uncommented by removing the # character from the beginning of the line.

This first set disables the CSS and JavaScript aggregation features.
```
$config['system.performance']['css']['preprocess'] = FALSE;
$config['system.performance']['js']['preprocess'] = FALSE;
```
2. And uncommenting this line effectively disables, or rather, bypasses the Render API cache:
```
$settings['cache']['bins']['render'] = 'cache.backend.null';
```
3. You can also disable the Dynamic Page Cache by uncommenting this line:
```
$settings['cache']['bins']['dynamic_page_cache'] = 'cache.backend.null';
```
**Enable Twig debugging options**
* Edit the following variables under the `twig.config:` section.

If you're placing this into your `sites/development.services.yml` file, add the `twig.config` configuration indented under the `parameters:` line. Ensure that your code additions are appropriately indented with 2 spaces, not the tab character (_or an error will result_).
```
parameters:
  twig.config:
    debug: true
    auto_reload: true
    cache: false
```
---------------------------------------------------------------------------------------------------------------------------------------------------------
## Installing themes or plugins ##
```
docker-compose exec php composer require 'drupal/<theme or plugin name>'
```




