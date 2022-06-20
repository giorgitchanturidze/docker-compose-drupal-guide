## Bitoid Docker4Drupal
# This is "Bitoid" dcoker4drupal installation instructions i have used for my project:

Docker4Drupal is an open-source project [GitHub page](https://github.com/wodby/docker4drupal) that provides pre-configured `docker-compose.yml` file with images to spin up local environment on Linux, Mac OS X and Windows.

# Requirements #
* [install Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).

> Note: The instructions below refer to the [global Composer installation](https://getcomposer.org/doc/00-intro.md#globally).
You might need to replace `composer` with `php composer.phar` (or similar) for your setup.

* Install Docker ([Linux](https://docs.docker.com/engine/install/), [Docker for Mac](https://docs.docker.com/desktop/mac/) or [Docker for Windows (10+ Pro)](https://docs.docker.com/desktop/windows/))

* For Linux additionally install [docker compose](https://docs.docker.com/compose/install/).


Make direction file for your project.
```
mkdir drupal-project
```

After that you can create the project:
```
composer create-project drupal-composer/drupal-project:9.x-dev drupal-project --no-interaction
```

Download and unpack `docker4drupal.tar.gz` from the [latest stable release](https://github.com/wodby/docker4drupal/releases) to your project root.

Delete `docker-compose.override.yml`.

Create configuration file.
```
mkdir config/sync -p
```

User permisions for container.
```
chown -R 1000:1000 ./
```

Optional: database acceess settings in your `.env` file:
```
MYSQL_DATABASE=${DB_NAME}
MYSQL_HOSTNAME=${DB_HOST}
MYSQL_PASSWORD=${DB_PASSWORD}
MYSQL_PORT=${DB_PORT}
MYSQL_USER=${DB_USER}
```

Optional: uncomment `13` and `15` lines in `docker-compose.yml` to prevent erasing database after `docker-compose down` command.









